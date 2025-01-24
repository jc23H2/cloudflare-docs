---
title: ​Purge cache by hostname
pcx_content_type: how-to
weight: 4
---

# ​Purge cache by hostname (Enterprise only)

Purging by hostname means that all assets at URLs with a host that matches one of the provided values will be purged from the cache.

1.  Log in to your [Cloudflare dashboard](https://dash.cloudflare.com/login), and select your account and domain.
2.  Select **Caching** > **Configuration**.
3.  Under **Purge Cache**, select **Custom Purge**. The **Custom Purge** window appears.
4.  Under **Purge by**, select **Hostname**.
5.  Follow the syntax instructions:
    - One hostname per line.
    - Separated by commas.
    - You can purge up to 30 hostnames at a time.
6.  Enter the appropriate value(s) in the text field using the format shown in the example.
7.  Select **Purge**.

{{<Aside type="note" header="API">}}

You can purge hostnames via the Cloudflare API. For more information, refer to the [API documentation](/api/operations/zone-purge). You can use up to 30 hostnames per API call and make up to 30,000 purge API calls in a 24-hour period.

{{</Aside>}}

## Resulting cache status

Purging by hostname deletes the resource, resulting in the `CF-Cache-Status` header being set to [`MISS`](/cache/concepts/cache-responses/#miss) for subsequent requests.

If [tiered cache](/cache/how-to/tiered-cache/) is used, purging by hostname may return `EXPIRED`, as the lower tier tries to revalidate with the upper tier to reduce load on the latter.
Depending on whether the upper tier has the resource or not, and whether the end user is reaching the lower tier or the upper tier, `EXPIRED` or `MISS` are returned.