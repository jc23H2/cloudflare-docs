---
title: Short-lived certificates
pcx_content_type: how-to
weight: 4
meta:
  title: Configure short-lived certificates
---

# Configure short-lived certificates

Cloudflare Access can replace traditional SSH key models with short-lived certificates issued to your users based on the token generated by their Access login. In traditional models, users generate a keypair and commit their public key into an infrastructure management tool, like [Salt](https://github.com/saltstack/salt), or otherwise upload it to an administrator. These keys can remain unchanged for months or years.

Cloudflare Access removes the burden on the end user of generating a key, while also improving security of access to infrastructure with ephemeral certificates.

## 1. Secure the server behind Cloudflare Access

Cloudflare Access short-lived certificates can work with any modern SSH server, whether it is behind Access or not. However, we recommend putting your server behind Access for added security and features, such as auditability and browser-based terminals.

To secure your server behind Cloudflare Access:

1. Connect the server to Cloudflare as a [public hostname route](/cloudflare-one/connections/connect-networks/use-cases/ssh/#1-connect-the-server-to-cloudflare-1).
2. Create a [self-hosted Access application](/cloudflare-one/applications/configure-apps/self-hosted-apps/) for the server.

{{<Aside type="note">}}
If you do not wish to use Access, refer instead to our [SSH proxy instructions](/cloudflare-one/policies/gateway/network-policies/ssh-logging/).
{{</Aside>}}

## 2. Ensure Unix usernames match user SSO identities

Cloudflare Access will take the identity from a token and, using short-lived certificates, authorize the user on the target infrastructure.

{{<render file="ssh/_usernames.md">}}

## 3. Generate a short-lived certificate public key

1. In Zero Trust, go to **Access** > **Service Auth** > **SSH**.

2. In the **Application** dropdown, choose the Access application that represents your SSH server.

3. Select **Generate certificate**. A row will appear with a public key scoped to your application.

4. Save the key or keep it somewhere convenient for configuring your server.
   You can return to copy this public key any time in the Service Auth dashboard.

## 4. Save your public key

1. Copy the public key generated from the dashboard in Step 3.

{{<render file="ssh/_public-key.md">}}

## 5. Modify your SSHD config

{{<render file="ssh/_modify-sshd.md">}}

## 6. Restart your SSH server

{{<render file="ssh/_restart-server.md">}}

## 7. Connect as a user

### Configure your client SSH config

On the client side, [configure your device](/cloudflare-one/connections/connect-networks/use-cases/ssh/) to use Cloudflare Access to reach the protected machine. To use short-lived certificates, you must include the following settings in your SSH config file (`~/.ssh/config`).

To save time, you can use the following cloudflared command to print the required configuration command:

```sh
$ cloudflared access ssh-config --hostname vm.example.com --short-lived-cert
```

If you prefer to configure manually, this is an example of the generated SSH config:

```bash
Match host vm.example.com exec "/usr/local/bin/cloudflared access ssh-gen --hostname %h"
    HostName vm.example.com
    ProxyCommand /usr/local/bin/cloudflared access ssh --hostname %h
    IdentityFile ~/.cloudflared/vm.example.com-cf_key
    CertificateFile ~/.cloudflared/vm.example.com-cf_key-cert.pub
```

### Connect through a browser-based terminal

End users can connect to the SSH session without any configuration by using Cloudflare's browser-based terminal. Users visit the URL of the application and Cloudflare's terminal handles the short-lived certificate flow. To enable, refer to [Enable browser rendering](/cloudflare-one/applications/non-http/#enable-browser-rendering).

---

Your SSH server is now protected behind Cloudflare Access — users will be prompted to authenticate with your identity provider before they can connect. You can also enable SSH command logging by configuring a [Gateway Audit SSH policy](/cloudflare-one/policies/gateway/network-policies/ssh-logging/).