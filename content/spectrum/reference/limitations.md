---
pcx_content_type: reference
title: Limitations
weight: 0
---

# Limitations

The following limitations apply to different protocols supported by Spectrum.

## HTTPS

At the moment, HTTPS applications do not support HTTP/3.

## UDP

At the moment, Cloudflare does not support packet fragmentation for UDP packets. If packets are fragmented, they will be dropped at Cloudflare’s edge.

## Minecraft

Minecraft Java Edition is supported but Minecraft Bedrock Edition is not supported.

## Universal SSL

[Universal SSL](/ssl/edge-certificates/universal-ssl/) is not compatible with Cloudflare Spectrum. Use either an [advanced certificate](/ssl/edge-certificates/advanced-certificate-manager/) or a [custom certificate](/ssl/edge-certificates/custom-certificates/) instead.

## Listen on ports configuration

By default, Spectrum is configured to listen on all ports, which can raise concerns for security auditors. However, it is important to note that Spectrum will only proxy connections from edge ports that are specifically configured within Cloudflare.

When a TCP handshake is initiated to any port for a Spectrum IP, the handshake will always be completed. If there is a Spectrum application configured for the port, the connection will be proxied to origin. If no application is configured, the connection is immediately terminated and no origin connection will be opened.

Spectrum will only ever proxy traffic to an origin if there is a Spectrum application configured for that port.