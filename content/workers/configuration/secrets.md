---
pcx_content_type: configuration
title: Secrets
meta:
  description: Store sensitive information, like API keys and auth tokens, in your Worker.
---

# Secrets

## Background

Secrets are a type of binding that allow you to attach encrypted text values to your Worker. You cannot see secrets after you set them and can only access secrets via [Wrangler](/workers/wrangler/commands/#secret) or programmatically via the [`env` parameter](/workers/runtime-apis/handlers/fetch/#parameters). Secrets are used for storing sensitive information like API keys and auth tokens. Secrets are available on the [`env` parameter](/workers/runtime-apis/handlers/fetch/#parameters) passed to your Worker's [`fetch` event handler](/workers/runtime-apis/handlers/fetch/).

## Add secrets to your project

### Secrets in development

{{<render file="_secrets-in-dev.md">}}

### Secrets on deployed Workers

#### Via Wrangler

To add a secret to a Worker, run the [`wrangler secret put` command](/workers/wrangler/commands/#secret) in your terminal, where `<KEY>` is the name of your secret:

```sh
---
filename: wrangler secret put
---
$ npx wrangler secret put <KEY>
```

#### Via the dashboard

To add a secret via the dashboard:

1. Log in to [Cloudflare dashboard](https://dash.cloudflare.com/) and select your account.
2. Select **Workers & Pages**.
3. In **Overview**, select your Worker > **Settings**.
4. Under **Environment Variables**, select **Add variable**.
5. Input a **Variable name** and its **value**, which will be made available to your Worker.
6. Select **Encrypt** to protect the secret's value. This will prevent the value from being visible via Wrangler and the dashboard.
7. (Optional) To add multiple secrets, select **Add variable**.
8. Select **Save** to implement your changes.

## Delete secrets from your project

### Via Wrangler

To delete a secret from your Worker project, run the [`wrangler secret delete` command](/workers/wrangler/commands/#delete-7):

```sh
---
filename: wrangler secret delete
---
$ npx wrangler secret delete <KEY>
```

### Via the dashboard

To delete a secret from your Worker project via the dashboard:

1. Log in to [Cloudflare dashboard](https://dash.cloudflare.com/) and select your account.
2. Select **Workers & Pages**.
3. In **Overview**, select your Worker > **Settings**.
4. Under **Environment Variables**, select **Edit variables**.
5. Select **X** next to the secret you want to delete.
6. Select **Save and deploy**.

{{<render file="_env_and_secrets.md">}}

## Related resources

* [Wrangler secret commands](/workers/wrangler/commands/#secret) - Review the Wrangler commands to create, delete and list secrets.