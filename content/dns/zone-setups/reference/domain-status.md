---
pcx_content_type: reference
title: Zone status
layout: wide
---

# Zone status

Review information on the different statuses that your [zone](/dns/concepts/#zone) can have after you [add your website or application](/fundamentals/setup/manage-domains/add-site/) to Cloudflare.

Zone status is also referred to as domain status. An **active** domain status is a requirement for your [application services configurations](/fundamentals/setup/manage-domains/connect-your-domain/#domain-configurations) to be applied. Refer to [How Cloudflare works](/fundamentals/concepts/how-cloudflare-works/) for details.

If your zone status changes, you will receive an email at the address associated with your account.

The following diagram gives you an overview of the different statuses applicable and how your zone may transition from one status to the other. For zones with an active paid subscription, the time to automatic deletion or purge may not correspond to this diagram. Refer to the sections below for details.

```mermaid
flowchart LR
accTitle: Zone status flow
accDescr: Diagram of the different statuses applicable to Cloudflare zones and the transitions from one status to the other.

A[Initializing]
B[Pending]
C[Active]
D[Moved]
E[Deleted]
F[Purged]

 A-- Plan <br />selection --> B
 B-- Zone <br />authentication --> C
 C-- DNS <br />checks fail --> D
 D-- Moved <br />for 7 days --> E
 E-- Deleted <br />for 7 days --> F

 B-- Pending for <br />28 days --> E
 A-- Initializing for 28 days --> E
```

{{<Aside type="note">}}
If you use the API to add your website or application to Cloudflare, your zone will be created directly in a **Pending** status. **Initializing** only applies to domains added via the dashboard.
{{</Aside>}}

## Initializing (Setup)

You have initiated the setup via dashboard, but did not select a plan for your zone. Your zone status is presented as **Setup** on the Cloudflare dashboard.

In this state, Cloudflare does not respond to any DNS queries for your domain.

If your zone is in **Setup** for over 28 days, it will be automatically [deleted](#deleted).

## Pending

Your zone status is presented as **Pending Nameserver Update** on the Cloudflare dashboard.

Cloudflare responds to DNS queries for pending zones on the assigned Cloudflare nameserver IPs, but your zone is still not active and cannot be used to [proxy traffic to Cloudflare](/dns/manage-dns-records/reference/proxied-dns-records/#pending-domains).

If your domain is on the Free plan, it will be deleted automatically if it is not activated within 28 days. Any pending zone with a paid plan (Pro, Business, Enterprise) will remain pending until the plan is removed or the domain is activated or [removed from Cloudflare](/fundamentals/setup/manage-domains/remove-domain/).

### Causes

- [Full setup](/dns/zone-setups/full-setup/): You have either not [changed your authoritative nameservers](/dns/nameservers/update-nameservers/) or your change has not yet been authenticated by Cloudflare.
- [Partial (CNAME) setup](/dns/zone-setups/partial-setup/): You have either not added the verification TXT record to your authoritative DNS provider or the record has not yet been authenticated by Cloudflare.

## Active

Cloudflare has authenticated your [nameserver changes](/dns/nameservers/update-nameservers/) or [verification TXT record](/dns/zone-setups/partial-setup/setup/#verify-ownership-for-your-domain) and you can proxy domain traffic through Cloudflare. For more details refer to [How Cloudflare works](/fundamentals/concepts/how-cloudflare-works/) and [Domain configurations](/fundamentals/setup/manage-domains/connect-your-domain/#domain-configurations).

## Moved

Your domain has failed multiple DNS checks, where either the Cloudflare nameservers are no longer present on your domain's `NS` records ([Full setup](/dns/zone-setups/full-setup/)) or no `SOA` record is returned for the zone ([Partial (CNAME) setup](/dns/zone-setups/partial-setup/)).

Zones that do not have any active paid subscriptions and have been moved will be deleted automatically after 7 days.

{{<Aside type="warning">}}
If you have an active paid subscription and no longer wish to use Cloudflare, make sure to also [manually remove your domain](/fundamentals/setup/manage-domains/remove-domain/).
{{</Aside>}}

## Deleted

Your zone has been archived. Cloudflare still responds to DNS queries for deleted zones on the assigned Cloudflare nameserver IPs (for non-deleted DNS records) and you can re-add the domain to Cloudflare by following the [regular onboarding flow](/fundamentals/setup/manage-domains/add-site/).

After being deleted for seven days, zones are automatically [purged](#purged).

## Purged

After a zone is deleted for seven days, it will be purged. Cloudflare does not respond to DNS queries for purged zones and, unlike [deleted zones](#deleted), this status cannot be reverted. In this case, even if you re-add the domain to the same Cloudflare account, none of the zone settings are expected to be restored.