# Route / DNS Investigator

Read-only MCP subagent for Cloudflare routing, DNS, custom domain, and workers.dev investigation.

## Worker

`afo-route-dns-investigator-mcp`

## Purpose

Live endpoint problems often look like Worker bugs but are actually route, DNS, or domain issues. This subagent separates routing/domain evidence from Worker code evidence.

It checks:

- Cloudflare zone resolution
- DNS records for a hostname
- Worker routes in a zone
- custom domains attached to Workers
- workers.dev fallback URL and subdomain status
- route conflicts where the most specific route points to another Worker
- custom domain conflicts where a hostname points to another Worker
- DNS proxy status and suspicious CNAME targets
- smoke-test behavior to see whether the endpoint appears to hit the intended Worker

## Tools

- `subagent_status`
- `resolve_zone`
- `list_dns_records`
- `list_worker_routes`
- `list_custom_domains`
- `get_workers_dev_url`
- `smoke_endpoint`
- `analyze_route_conflicts`
- `investigate_route_dns`

## Runtime credentials

Live Cloudflare inspection requires a read-only Cloudflare API credential exposed to the Worker as one of:

- `CLOUDFLARE_API_TOKEN`
- `CF_API_TOKEN`
- `CLOUDFLARE_TOKEN`

Also provide the account id either as a Worker variable or per tool call:

- `CLOUDFLARE_ACCOUNT_ID`
- `CF_ACCOUNT_ID`
- `ACCOUNT_ID`

Zone-scoped checks need either `zone_id`, `zone_name`, `domain`, `CF_ZONE_ID`, `DEFAULT_ZONE_ID`, or `DEFAULT_ZONE_NAME`.

## Safety boundary

This Worker only performs Cloudflare GET requests and HTTP smoke checks. It does not create, update, delete, deploy, rerun, mutate DNS, mutate routes, or alter traffic.
