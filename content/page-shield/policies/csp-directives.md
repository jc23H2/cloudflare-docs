---
title: Supported CSP directives
pcx_content_type: reference
weight: 6
meta:
  description: CSP directives supported by policies
---

# CSP directives supported by policies

Page Shield policies support most {{<glossary-tooltip term_id="content security policy (CSP)">}}Content Security Policy (CSP){{</glossary-tooltip>}} directives, covering both monitored and unmonitored resources. You can use a policy to control other types of resources besides scripts and their connections, even though Page Shield is not monitoring these resources.

Each CSP directive can contain multiple values, including:
- Schemes
- Hostnames
- URIs
- Special keywords between single quotes (for example, `'none'`)
- Hashes between single quotes (for example, `'sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC'`)

Hostname and URI values support a `*` wildcard for the leftmost subdomain.

The following table lists the supported CSP directives and special values you can use in Page Shield policies:

{{<table-wrap>}}

Directive         | Name in the dashboard | Supported special values | Monitored
------------------|-----------------------|--------------------------|----------
`script-src`      | Scripts         | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | [Yes](/page-shield/detection/monitor-connections-scripts/)
`connect-src`     | Connections     | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | [Yes](/page-shield/detection/monitor-connections-scripts/)
`default-src`     | Default         | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`img-src`         | Images          | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`style-src`       | Styles          | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`font-src`        | Fonts           | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`object-src`      | Objects         | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`media-src`       | Media           | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`child-src`       | Child           | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`form-action`     | Form actions    | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`worker-src`      | Workers         | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`base-uri`        | Base URI        | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`manifest-src`    | Manifests       | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`frame-src`       | Frames          | `'none'`<br>`'self'`<br>`'unsafe-inline'`<br>`'unsafe-eval'`<br>`'<HASH>'` | No
`frame-ancestors` | Frame ancestors | `'none'`<br>`'self'`           | No
`upgrade-insecure-requests` | Upgrade insecure requests | N/A        | No

{{</table-wrap>}}

## More resources

For more information on CSP directives and their values, refer to the following resources in the MDN documentation:
* [Content-Security-Policy response header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)
* [CSP source values](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/Sources)