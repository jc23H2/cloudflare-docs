---
pcx_content_type: faq
title: FAQ
weight: 5
structured_data: true
---

# FAQ

Below you will find answers to our most commonly asked questions. If you cannot find the answer you are looking for, refer to the [community page](https://community.cloudflare.com/c/website-application-performance/88) to explore more resources.

{{<faq-item>}}
{{<faq-question level=2 text="How long does it take for a test to load?" >}}

{{<faq-answer>}}

It can vary from about 25 seconds to over a minute. If you leave your speed tab open, your test is still going to run. You can leave and return and still see your test results.

{{</faq-answer>}}
{{</faq-item>}}

{{<faq-item>}}
{{<faq-question level=2 text="I get a `403` response when rerunning the website analysis?" >}}

{{<faq-answer>}}

Check your WAF custom rules to make sure that you are not blocking traffic from Observatory to request your site.

{{<render file="_user-agents.md">}}

{{</faq-answer>}}
{{</faq-item>}}

{{<faq-item>}}
{{<faq-question level=2 text="Why might users not see any Real User Monitoring (RUM) data on the map in Observatory?" >}}

{{<faq-answer>}}

There are several reasons why users might not see any Real User Monitoring (RUM) data on the map in Observatory:

- Limited Traffic to Specific URL Paths: When users see the map in Observatory, they are filtering data by a specific URL path. If there is no traffic to that particular path, there will be no corresponding RUM data available to display on the map.

- Time Required for RUM Data Population: Populating the RUM database takes some time. It means that newly enabled RUM might not have immediate data available, and users may need to wait for some time before RUM data starts appearing on the map.

- Progressive Sampling: RUM data is progressively sampled, which means that not all requests are captured. Some requests may pass through the sampling period, resulting in incomplete or missing data points on the map.

- Adblockers Impact on RUM Data: RUM data collection relies on third-party JavaScript executing on the real-user browser. However, adblockers or similar browser extensions can block this script, preventing the collection of RUM data, and thereby affecting the completeness of the analytics presented on the map.

{{</faq-answer>}}
{{</faq-item>}}

{{<faq-item>}}
{{<faq-question level=2 text="What are the potential reasons for discrepancies between RUM analytics and traffic analytics in Observatory?" >}}

{{<faq-answer>}}

Differences between Real User Monitoring (RUM) analytics and traffic analytics in Observatory can occur due to the following reasons:

- Adblockers Impact on RUM Data: Similar to the previous point, RUM data collection can be thwarted by adblockers, leading to missed data. Since traffic analytics typically rely on server-side data collection, they may not be as affected by adblockers as RUM.

- Progressive Sampling in RUM: RUM data is collected through progressive sampling, which means that not all user requests are captured. This sampling method could result in slight variations in analytics when compared to traditional traffic analytics that record every server request.

{{</faq-answer>}}
{{</faq-item>}}

{{<faq-item>}}
{{<faq-question level=2 text="How do I disable Real User Monitoring (RUM) if it has been enabled from the Observatory test result page?" >}}

{{<faq-answer>}}

Enabling RUM creates a Web Analytics configuration entry for the hostname at the account level, under **Analytics & Logs > Web Analytics**.

If you wish to disable RUM, follow these steps:

1.  Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/login) and select your account.
2.  Go to **Analytics & Logs > Web Analytics**.
3.  Select **Manage Site** for the hostname for which you wish to disable RUM.
4.  Select **Delete**.
{{</faq-answer>}}
{{</faq-item>}}