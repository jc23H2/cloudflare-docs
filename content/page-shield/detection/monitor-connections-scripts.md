---
title: Monitor resources and cookies
pcx_content_type: how-to
weight: 2
meta:
  title: Monitor resources and cookies
---

# Monitor resources and cookies

Once you [activate Page Shield](/page-shield/get-started/), the **Monitors** dashboard will show which resources (scripts and connections) are running on your domain, as well as the cookies recently detected in HTTP traffic.

If you notice unexpected scripts or connections on the dashboard, check them for signs of malicious activity. Enterprise customers with a paid add-on will have their [connections and scripts classified as potentially malicious](/page-shield/how-it-works/malicious-script-detection/) based on threat feeds. You should also check for any new or unexpected cookies.

{{<Aside type="note">}}
If you recently activated Page Shield, you may see a delay in reporting.
{{</Aside>}}

## Use the Monitors dashboard

To review the resources and cookies detected by Page Shield:

1.  Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/), and select your account and domain.
2.  Go to **Security** > **Page Shield**.
3.  Under **Monitors**, review the list of scripts, connections, and cookies for your domain. To apply a filter, select **Add filter** and use one or more of the available options:

    - **Script**: Filter scripts by their URL.
    - **Connection**: Filter connections by their target URL. Depending on your [configuration](/page-shield/reference/settings/#connection-target-details), it may search only by target hostname.
    - **Host**: Look for scripts appearing on specific hostnames, or connections made in a specific hostname.
    - **Page** (requires a Business or Enterprise plan): Look for scripts appearing in a specific page, or for connections made in a specific page. Searches the first page where the script was loaded (or where the connection was made) and the latest occurrences list.
    - **Status**: Filter scripts or connections by [status](/page-shield/reference/script-statuses/).
    - **Type**: Filter cookies according to their type: first-party cookies or unknown.
    - Cookie property: Filter by a cookie property such as **Name**, **Domain**, **Path**, **Same site**, **HTTP only**, and **Secure**.

5. Depending on your plan, you may be able to [view the details of each item](#view-details).

## View all reported scripts or connections

The All Reported Connections and All Reported Scripts dashboards show all the detected resources including infrequent or inactive ones, reported in the last 30 days. After 30 days without any report, Page Shield will delete information about a previously reported resource, and it will no longer appear in any of the dashboards.

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/), and select your account and domain.
2. Go to **Security** > **Page Shield** > **Monitors**.
3. Select **Scripts** or **Connections**.
3. Select **View all**.
4. Review the information displayed in the dashboard.

You can filter the data in these dashboards using different criteria, and print a report with the displayed records.

## View details

{{<Aside type="note">}}
Only available to customers on Business and Enterprise plans.
{{</Aside>}}

To view the details of an item, select **Details** next to the item.

The details of each connection or script include:

- **Last seen**: How long ago the resource was last detected (in the last 30 days).
- **First seen at**: The date and time when the resource was first detected.
- **Page URLs**: The most recent pages where the resource was detected (up to ten pages).
- **First page URL**: The page where the resource was first detected.
- **Host**: The host where the script is being loaded or the connection is being made.

The details of each cookie include:

- **Type**: A cookie can have the following types:

    - **First-party**: Cookies set by the origin server through a `set-cookie` HTTP response header.
    - **Unknown**: All other detected cookies.

- **Domain**: The value of the `Domain` cookie attribute. When not set or unknown, this value is derived from the host.
- **Path**: The value of the `Path` cookie attribute. When not set or unknown, this value is derived from the most recent page where the cookie was detected.
- **Last seen**: How long ago the resource was last detected (in the last 30 days).
- **First seen at**: The date and time when the cookie was first detected.
- **Seen on host**: The host where the cookie was first detected.
- **Seen on pages**: The most recent pages where the cookie was detected (up to ten pages).

- Additional cookie attributes (only available to Enterprise customers with a paid add-on):
    - **Max age**: The value of the `Max-Age` cookie attribute.
    - **Expires**: The value of the `Expires` cookie attribute.
    - **Lifetime**: The approximate cookie lifetime, based on the `Max-Age` and `Expires` cookie attributes.
    - **HTTP only**: The value of the `HttpOnly` cookie attribute.
    - **Secure**: The value of the `Secure` cookie attribute.
    - **Same site**: The value of the `SameSite` cookie attribute.

Except for **Domain** and **Path**, [standard cookie attributes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) are only available for first-party cookies, where Cloudflare detected the `set-cookie` HTTP response header in HTTP traffic.

## Export data

{{<Aside type="note">}}
Only available to Enterprise customers with a paid add-on.
{{</Aside>}}

Use this feature to extract data from Page Shield that you can review and annotate. The data in the exported file will honor any filters you configure in the dashboard.

To export script, connection, or cookie information in CSV format:

1. Go to the **Monitors** tab and select **Scripts**, **Connections**, or **Cookies**.
2. (Optional) Apply any filters to the displayed data.
3. Select **Download CSV**.