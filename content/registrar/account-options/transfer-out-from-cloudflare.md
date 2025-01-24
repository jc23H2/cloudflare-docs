---
pcx_content_type: how-to
title: Transfer domain out from Cloudflare
meta:
    title: Transfer domain from Cloudflare to another registrar
---

# Transfer domain from Cloudflare to another registrar

Cloudflare Registrar makes it easy to transfer your domain to another registrar. Be aware that ICANN rules prohibit a domain from being transferred if:

- The domain has been transferred within the last 60 days;
- The domain was registered within the last 60 days;
- If the WHOIS registrant information has been modified in the last 60 days (even if redacted).

Follow the instructions below to transfer your domain out from Cloudflare.

{{<Aside type="warning" header="Warning">}}
Anyone with super-admin and admin permissions for a zone can also manage your domains. This means these users can also unlock domains or obtain authorization codes to transfer domains to other registrars. Be careful who you give these account roles to.
{{</Aside>}} 

## 1. Unlock your domain at Cloudflare

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/login) and select your account.
2. Go to **Domain Registration** > **Manage Domains**.
3. Find the domain you want to transfer, and select **Manage**.
4. Select **Configuration** > **Unlock**.
5. Select **Confirm and Unlock** to confirm that you want to unlock your domain.
6. Copy the auth code (also referred to as authentication code and authorization code) generated by Cloudflare, and use at your new registrar.

If you lose your authentication code, you can get a new one by:
* Selecting the **Regenerate** button;
* Locking the domain and repeating steps 1-6.

## 2. Transfer to a new registrar

1. Go to your new registrar.
2. You will be asked for the authorization code from Cloudflare (it might be called EPP in some systems). Input the code created for you from the Cloudflare dashboard.
3. Your new registrar will send the transfer request to the registry for your domain. The registry will then send it to Cloudflare. After Cloudflare receives the message, you can manually approve the transfer to initiate it immediately.
4. You will need to confirm the approval. You can also reject it at this stage. If you reject it, Cloudflare will reapply the registrar lock.
5. If you do not manually approve the transfer, the transfer will auto-approve on the fifth day after receiving the request. In either case, when your transfer out completes Cloudflare will remove the domain from your account and you will not be charged for future renewals.