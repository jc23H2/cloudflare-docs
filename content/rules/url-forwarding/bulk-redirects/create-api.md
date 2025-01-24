---
pcx_content_type: how-to
title: Create via API
weight: 7
meta:
  title: Create Bulk Redirects via API
  description: Learn how to create Bulk Redirects using the Cloudflare API.
---

# Create Bulk Redirects via API

To create Bulk Redirects via API, you must:

1. Create a Bulk Redirect List via API.
2. Add items (URL redirects) to the list created in step 1.
3. Create a Bulk Redirect Rule via API, which enables the list created in step 1.

{{<render file="url-forwarding/_requires-proxied-site.md" withParameters="Bulk Redirects">}}

## 1. Create a Bulk Redirect List via API

Use the [Create a list](/api/operations/lists-create-a-list) operation to create a new Bulk Redirect List. The list `kind` must be `redirect`.

```bash
---
header: Request
---
curl https://api.cloudflare.com/client/v4/accounts/{account_id}/rules/lists \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "name": "my_redirect_list",
  "description": "My redirect list.",
  "kind": "redirect"
}'
```

The response will be similar to the following:

```json
---
header: Response
highlight: 3
---
{
  "result": {
    "id": "f848b6ccb07647749411f504d6f88794",
    "name": "my_redirect_list",
    "description": "My redirect list.",
    "kind": "redirect",
    "num_items": 0,
    "num_referencing_filters": 0,
    "created_on": "2021-10-28T09:11:42Z",
    "modified_on": "2021-10-28T09:11:42Z"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

Take note of the list ID — you will need it in the next step.

For more information on list operations, refer to the [Lists API](/waf/tools/lists/lists-api/) documentation.

## 2. Add items to the list

Use the [Create list items](/api/operations/lists-create-list-items) operation to add URL redirect items to the list. Enter the list ID from the previous step in the endpoint URL:

```bash
---
header: Request
---
curl https://api.cloudflare.com/client/v4/accounts/{account_id}/rules/lists/f848b6ccb07647749411f504d6f88794/items \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '[
  {
    "redirect": {
      "source_url": "example.com/blog/",
      "target_url": "https://example.com/blog/latest"
    }
  },
  {
    "redirect": {
      "source_url": "example.net/",
      "target_url": "https://example.net/under-construction.html",
      "status_code": 307
    }
  }
]'
```

The response will be similar to the following:

```json
---
header: Response
---
{
  "result": {
    "operation_id": "92558f8b296d4dbe9d0419e0e53f6622"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

This is an asynchronous operation. The response will contain an `operation_id` which you will use to check if the operation completed successfully using the [Get bulk operation status](/api/operations/lists-get-bulk-operation-status) operation:

```bash
---
header: Request
---
curl https://api.cloudflare.com/client/v4/accounts/{account_id}/rules/lists/bulk_operations/92558f8b296d4dbe9d0419e0e53f6622 \
--header "Authorization: Bearer <API_TOKEN>"
```

If the operation already completed successfully, the response will be similar to the following:

```json
---
header: Response
---
{
  "result": {
    "id": "92558f8b296d4dbe9d0419e0e53f6622",
    "status": "completed",
    "completed": "2021-10-28T09:15:42Z"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

## 3. Create a Bulk Redirect Rule via API

Since Bulk Redirect Lists are essentially containers of URL redirects, you have to enable the URL redirects in the list by creating a Bulk Redirect Rule.

Add Bulk Redirect Rules to the [entry point ruleset](/ruleset-engine/about/rulesets/#entry-point-ruleset) of the `http_request_redirect` phase at the account level. Refer to the [Rulesets API](/ruleset-engine/rulesets-api/) documentation for more information on [creating a ruleset](/ruleset-engine/rulesets-api/create/) and supplying a list of rules for the ruleset.

A Bulk Redirect Rule must have:

* `action` set to `redirect`
* An `action_parameters` object with additional configuration settings — refer to [API JSON objects: Bulk Redirect Rule](/rules/url-forwarding/bulk-redirects/reference/json-objects/#bulk-redirect-rule) for details.

The following request of the [Create an account ruleset](/api/operations/createAccountRuleset) operation creates a phase entry point ruleset for the `http_request_redirect` phase at the account level, and defines a single redirect rule. Use this operation if you have not created a phase entry point ruleset for the `http_request_redirect` phase yet.

```bash
---
header: Request
---
curl https://api.cloudflare.com/client/v4/accounts/{account_id}/rulesets \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "name": "My redirect ruleset",
  "kind": "root",
  "phase": "http_request_redirect",
  "rules": [
    {
      "expression": "http.request.full_uri in $my_redirect_list",
      "description": "Bulk Redirect rule.",
      "action": "redirect",
      "action_parameters": {
        "from_list": {
          "name": "my_redirect_list",
          "key": "http.request.full_uri"
        }
      }
    }
  ]
}'
```

The response will be similar to the following:

```json
---
header: Response
---
{
  "result": {
    "id": "528f4f03bf0da53a29907199625867be",
    "name": "My redirect ruleset",
    "kind": "root",
    "version": "1",
    "rules": [
      {
        "id": "8da312df846b4258a05bcd454ea943be",
        "version": "1",
        "expression": "http.request.full_uri in $my_redirect_list",
        "description": "Bulk Redirect rule.",
        "action": "redirect",
        "action_parameters": {
          "from_list": {
            "name": "my_redirect_list",
            "key": "http.request.full_uri"
          }
        },
        "last_updated": "2021-10-28T09:20:42Z",
      }
    ],
    "last_updated": "2021-10-28T09:20:42Z",
    "phase": "http_request_redirect"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

If there is already a phase entry point ruleset for the `http_request_redirect` phase, use the [Update an account ruleset](/api/operations/updateAccountRuleset) operation instead, like in the following example:

```bash
---
header: Request
---
curl --request PUT \
https://api.cloudflare.com/client/v4/accounts/{account_id}/rulesets/{ruleset_id} \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "name": "My redirect ruleset",
  "kind": "root",
  "phase": "http_request_redirect",
  "rules": [
    {
      "expression": "http.request.full_uri in $my_redirect_list_2",
      "description": "Bulk Redirect rule 1",
      "action": "redirect",
      "action_parameters": {
        "from_list": {
          "name": "my_redirect_list_1",
          "key": "http.request.full_uri"
        }
      }
    },
    {
      "expression": "http.request.full_uri in $my_redirect_list_2",
      "description": "Bulk Redirect rule 2",
      "action": "redirect",
      "action_parameters": {
        "from_list": {
          "name": "my_redirect_list_2",
          "key": "http.request.full_uri"
        }
      }
    }
  ]
}'
```

The response will be similar to the following:

```json
---
header: Response
---
{
  "result": {
    "id": "67013aa153df4e5fbda92f92bc979331",
    "name": "default",
    "description": "",
    "kind": "root",
    "version": "2",
    "rules": [
      {
        "id": "8be62ab2ef9a4a41af30c24ff8e73e41",
        "version": "1",
        "action": "redirect",
        "action_parameters": {
          "from_list": {
            "name": "my_redirect_list_1",
            "key": "http.request.full_uri"
          }
        },
        "expression": "http.request.full_uri in $my_redirect_list_1",
        "description": "Bulk Redirect rule 1",
        "last_updated": "2021-12-03T15:38:51.658387Z",
        "ref": "8be62ab2ef9a4a41af30c24ff8e73e41",
        "enabled": true
      },
      {
        "id": "97e38797fb2b4b22a4919800f1318a5c",
        "version": "1",
        "action": "redirect",
        "action_parameters": {
          "from_list": {
            "name": "my_redirect_list_2",
            "key": "http.request.full_uri"
          }
        },
        "expression": "http.request.full_uri in $my_redirect_list_2",
        "description": "Bulk Redirect rule 2",
        "last_updated": "2021-12-03T15:38:51.658387Z",
        "ref": "97e38797fb2b4b22a4919800f1318a5c",
        "enabled": true
      }
    ],
    "last_updated": "2021-12-03T15:38:51.658387Z",
    "phase": "http_request_redirect"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

---

## Required API token permissions

The API token used in API requests to manage Bulk Redirects objects (lists, list items, and rules) must have at least the following permissions:

* _Account_ > _Account Rulesets_ > _Edit_
* _Account_ > _Account Filter Lists_ > _Edit_