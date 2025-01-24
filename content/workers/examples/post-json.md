---
type: example
summary: Send a POST request with JSON data. Use to share data with external servers.
tags:
  - JSON
languages:
  - JavaScript
  - TypeScript
  - Python
pcx_content_type: example
title: Post JSON
weight: 1001
layout: example
---

{{<tabs labels="js | ts | py">}}
{{<tab label="js" default="true">}}

```js
export default {
  async fetch(request) {
    /**
     * Example someHost is set up to take in a JSON request
     * Replace url with the host you wish to send requests to
     * @param {string} url the URL to send the request to
     * @param {BodyInit} body the JSON data to send in the request
     */
    const someHost = "https://examples.cloudflareworkers.com/demos";
    const url = someHost + "/requests/json";
    const body = {
      results: ["default data to send"],
      errors: null,
      msg: "I sent this to the fetch",
    };

    /**
     * gatherResponse awaits and returns a response body as a string.
     * Use await gatherResponse(..) in an async function to get the response body
     * @param {Response} response
     */
    async function gatherResponse(response) {
      const { headers } = response;
      const contentType = headers.get("content-type") || "";
      if (contentType.includes("application/json")) {
        return JSON.stringify(await response.json());
      } else if (contentType.includes("application/text")) {
        return response.text();
      } else if (contentType.includes("text/html")) {
        return response.text();
      } else {
        return response.text();
      }
    }

    const init = {
      body: JSON.stringify(body),
      method: "POST",
      headers: {
        "content-type": "application/json;charset=UTF-8",
      },
    };
    const response = await fetch(url, init);
    const results = await gatherResponse(response);
    return new Response(results, init);
  },
};
```

{{</tab>}}
{{<tab label="ts">}}

```ts
export default {
  async fetch(request): Promise<Response> {
    /**
     * Example someHost is set up to take in a JSON request
     * Replace url with the host you wish to send requests to
     * @param {string} url the URL to send the request to
     * @param {BodyInit} body the JSON data to send in the request
     */
    const someHost = "https://examples.cloudflareworkers.com/demos";
    const url = someHost + "/requests/json";
    const body = {
      results: ["default data to send"],
      errors: null,
      msg: "I sent this to the fetch",
    };

    /**
     * gatherResponse awaits and returns a response body as a string.
     * Use await gatherResponse(..) in an async function to get the response body
     * @param {Response} response
     */
    async function gatherResponse(response) {
      const { headers } = response;
      const contentType = headers.get("content-type") || "";
      if (contentType.includes("application/json")) {
        return JSON.stringify(await response.json());
      } else if (contentType.includes("application/text")) {
        return response.text();
      } else if (contentType.includes("text/html")) {
        return response.text();
      } else {
        return response.text();
      }
    }

    const init = {
      body: JSON.stringify(body),
      method: "POST",
      headers: {
        "content-type": "application/json;charset=UTF-8",
      },
    };
    const response = await fetch(url, init);
    const results = await gatherResponse(response);
    return new Response(results, init);
  },
} satisfies ExportedHandler;
```

{{</tab>}}
{{<tab label="py">}}

```py
import json
from pyodide.ffi import to_js as _to_js
from js import Object, fetch, Response, Headers

def to_js(obj):
    return _to_js(obj, dict_converter=Object.fromEntries)

# gather_response returns both content-type & response body as a string
async def gather_response(response):
    headers = response.headers
    content_type = headers["content-type"] or ""

    if "application/json" in content_type:
        return (content_type, json.dumps(dict(await response.json())))
    return (content_type, await response.text())

async def on_fetch(_request):
    url = "https://jsonplaceholder.typicode.com/todos/1"

    body = {
      "results": ["default data to send"],
      "errors": None,
      "msg": "I sent this to the fetch",
    }

    options = {
      "body": json.dumps(body),
      "method": "POST",
      "headers": {
        "content-type": "application/json;charset=UTF-8",
      },
    }

    response = await fetch(url, to_js(options))
    content_type, result = await gather_response(response)

    headers = Headers.new({"content-type": content_type}.items())
    return Response.new(result, headers=headers)
```

{{</tab>}}
{{</tabs>}}