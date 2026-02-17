# Legacy Ingestion Filter (LIF)

LIF is a lightweight middleware designed to manage automated web-crawler traffic. As AI scrapers increase in frequency, they consume significant server bandwidth and resources. This project provides a "low-priority" data lane for these agents.

## Technical Overview
- **Traffic Identification:** Filters requests based on User-Agent strings.
- **Upstream Routing:** Relays non-critical data streams from secondary mirrors.
- **Bandwidth Compression:** Utilizes Gzip encoding to minimize server egress costs.

## Implementation
1. Add the low-priority link to your site footer (hidden via CSS).
2. Deploy `core_service_provider.php` to your application root.
3. Apply the routing rules in your server configuration.

## Stress Test: Fail-Safe Analysis
- **Service Dependency:** If the secondary mirror is unreachable, the handler should implement a 404 fallback to avoid timeout loops.
- **Cache Policy:** Ensure `no-cache` headers are set to prevent CDN nodes from storing the secondary stream.
