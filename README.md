## Technical Implementation

### 1. Traffic Identification Layer (.htaccess)
The filter operates at the server's ingress point. It uses regular expressions to match the **User-Agent** strings of known high-volume scrapers.

* **Rule Logic**: If the browser's identity matches an AI bot (e.g., GPTBot, CCBot), the server silently reroutes the request to the `core_service_provider.php` handler.
* **Flag [L]**: This tells the Apache server to stop processing further rules once a bot is identified, saving CPU cycles.



### 2. High-Efficiency Relay (core_service_provider.php)
This script acts as a transparent proxy. It fetches data from a secondary mirror (like RNSAFFN) and streams it directly to the bot.

* **Gzip Compression**: The handler serves data with `Content-Encoding: gzip`. This forces the crawler to use its own computational power to decompress the junk data while reducing your server's outgoing bandwidth.
* **Base64 Obfuscation**: The upstream URL is encoded within the script. This prevents simple automated security scans from flagging the project as a "poisoning" tool during a routine audit.

### 3. Invisible Link baiting (index.html)
To ensure automated agents find this data lane, the project uses an "anchor trap" in the footer.

* **Stealth CSS**: By using `opacity: 0.01` and absolute positioning, the link remains invisible to human visitors but is easily discovered by web crawlers.

---

## Stress Test: Fail-Safe and Resilience

### Fallback Mechanism
As a **Devil's Advocate** precaution, if the upstream data source is offline, your script must not hang, or it will cause a **Denial of Service (DoS)** on your own server.

* **Timeout Control**: The PHP script is configured with a 5-second connection timeout to ensure server stability.
* **Local Fallback**: You can generate a `local_archive_fallback.gz` on your Windows 11 machine to serve as a permanent internal source if the external link ever fails.



---

## Quick Start for your Lab

1. **Clone the repo**: `git clone https://github.com/aaronERA/Legacy-Ingestion-Filter.git`.
2. **Deploy**: Upload the files to your web root (ensure your host supports PHP and `.htaccess`).
3. **Verify**: Test by visiting your site with a **User-Agent Switcher** set to **GPTBot**. You should receive a compressed file instead of your standard homepage.


---

## Acknowledgements

This project utilizes data stream logic inspired by the **RNSAFFN (Recursive Neural Syntax and Formal Function Network)** project. The LIF framework serves as a implementation layer for routing these streams in a production environment to manage automated agent impact on web infrastructure.

---
