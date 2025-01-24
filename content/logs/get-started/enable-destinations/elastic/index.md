---
title: Enable Elastic
pcx_content_type: how-to
weight: 59
meta:
  title: Enable Logpush to Elastic
---

# Enable Logpush to Elastic

Push your Cloudflare logs to Elastic for instant visibility and insights. Enabling this integration with Elastic comes with a predefined dashboard to view all of your Cloudflare observability and security data with ease.

The Cloudflare Logpush integration can be used in three different modes to collect data:

- **HTTP Endpoint mode** - Cloudflare pushes logs directly to an HTTP endpoint hosted by your Elastic Agent.
- **AWS S3 polling mode** - Cloudflare writes data to S3, and the Elastic Agent polls the S3 bucket by listing its contents and reading new files.
- **AWS S3 SQS mode** - Cloudflare writes data to S3, S3 pushes a new object notification to SQS, the Elastic Agent receives the notification from SQS, and then reads the S3 object. Multiple Agents can be used in this mode.

{{<Aside type="note" header="Note">}}

Elastic recommends the AWS S3 SQS mode.

{{</Aside>}}

## Enable Logpush Job in Cloudflare

Determine which method you want to use, and configure the appropriate Logpush job in the Cloudflare dashboard or via the API.

Elastic supports the default JSON format.

To push logs to an object storage for short term storage and buffering before ingesting into Elastic (recommended), follow the instructions to configure a Logpush job to push logs to [AWS S3](/logs/get-started/enable-destinations/aws-s3/), [Google Cloud Storage](/logs/get-started/enable-destinations/google-cloud-storage/), or [Azure Blob Storage](/logs/get-started/enable-destinations/azure/).

To use the [HTTP Endpoint mode](/logs/get-started/enable-destinations/http/), use the API to push logs to an HTTP endpoint backed by your Elastic Agent.

Add the same custom header along with its value on both sides for additional security.

For example, while creating a job along with a header and value for a particular dataset:

```bash
curl --location --request POST 'https://api.cloudflare.com/client/v4/zones/<ZONE ID>/logpush/jobs' \
--header 'X-Auth-Key: <X-AUTH-KEY>' \
--header 'X-Auth-Email: <X-AUTH-EMAIL>' \
--header 'Authorization: <BASIC AUTHORIZATION>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"<public domain>",
    "destination_conf": "https://<public domain>:<public port>?header_<secret_header>=<secret_value>",
    "dataset": "http_requests",
    "logpull_options": "fields=RayID,EdgeStartTimestamp&timestamps=rfc3339"
}'
```

## Enable the Integration in Elastic

Once the Logpush job is configured, follow Elastics instructions for [setting up the Integration](https://docs.elastic.co/integrations/cloudflare_logpush) in the Elastic app.

## View Dashboards

Log in to your [Elastic account](https://www.elastic.co/) to view prebuilt dashboards and configure alerts.