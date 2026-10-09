[中文版](README.md)

# httpx — Bulk Web Reconnaissance Tool

httpx is a high-speed web reconnaissance tool developed by ProjectDiscovery (written in Go). It can probe large numbers of URLs in bulk and quickly retrieve status codes, page titles, tech stacks, server versions and more, helping you pick out targets worth investigating further during a penetration test.

## Questions httpx Can Answer

| Info Type | Parameter | Description |
|---------|------|------|
| Status code | `-status-code` | HTTP status code returned by the target (200, 301, 403, 500, etc.) |
| Page title | `-title` | Content of the page's `<title>` tag |
| Tech detection | `-tech-detect` | Frameworks/CMS used (WordPress, React, Nginx, etc.) |
| Server | `-web-server` | Web server software and version |
| IP address | `-ip` | Resolved IP address |
| CDN detection | `-cdn` | Whether a CDN is used (Cloudflare, Akamai, etc.) |
| Content length | `-content-length` | Content-Length of the response |
| TLS certificate | `-tls-grab` | TLS certificate info (issuer, expiration date, etc.) |

## Core Value — Bulk Processing

Difference from `curl`:

```
curl → can only probe one URL at a time, you have to write your own loop
httpx → can handle thousands of targets at once, with automatic concurrency and filtering
```

When subfinder finds thousands of subdomains, httpx can sort out within minutes which ones are alive, what services they run, and what technologies they use, saving you the time of checking them one by one by hand.

## Where It Fits in the Pentest Workflow

```
subfinder / amass         httpx                  gobuster / nmap
(subdomain enum)   →    (bulk probe/filter)  →   (deep scan)
                          │
                          ├─ Which subdomains are alive?
                          ├─ What web services are running?
                          └─ What technologies are used?
                                    │
                                    ▼
                          Pick out high-value targets
                          (e.g. outdated CMS, test environments, admin panels)
```

## Installation

### Docker Installation

```bash
docker pull projectdiscovery/httpx:latest
```

### Go Installation

```bash
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

## Common Commands

### Basic Usage — Single URL

```bash
echo "https://example.com" | docker run --rm -i projectdiscovery/httpx
```

### Basic Usage — Read from a File

```bash
docker run --rm -i -v $(pwd):/data projectdiscovery/httpx -l /data/urls.txt
```

### Using with subfinder (Pipeline)

```bash
docker run --rm -i projectdiscovery/subfinder -d example.com -silent | \
  docker run --rm -i projectdiscovery/httpx -silent
```

### Common Parameters

| Parameter | Description |
|------|------|
| `-status-code` (`-sc`) | Show HTTP status code |
| `-title` | Show page title |
| `-tech-detect` (`-td`) | Detect technologies/frameworks used |
| `-web-server` | Show web server info |
| `-ip` | Show resolved IP |
| `-cdn` | Detect whether a CDN is used |
| `-content-length` (`-cl`) | Show response content length |
| `-tls-grab` | Grab TLS certificate info |
| `-o <file>` | Output results to a file |
| `-silent` | Silent mode, only output results |
| `-threads <n>` | Set concurrency (default 50) |
| `-follow-redirects` (`-fr`) | Follow redirects |
| `-mc <code>` | Only show results matching the specified status code |
| `-fc <code>` | Filter out results with the specified status code |

### Multi-Parameter Examples

Get status code, title, tech stack, and server info all at once:

```bash
docker run --rm -i -v $(pwd):/data projectdiscovery/httpx \
  -l /data/subdomains.txt \
  -status-code -title -tech-detect -web-server \
  -o /data/httpx_result.txt
```

With subfinder, list only targets that respond with 200:

```bash
docker run --rm -i projectdiscovery/subfinder -d example.com -silent | \
  docker run --rm -i projectdiscovery/httpx -silent \
  -status-code -title -tech-detect \
  -mc 200
```

## Notes

- Only probe targets you are authorized to test; scanning other people's systems without authorization is illegal in most countries
- httpx's default concurrency is 50, so keep an eye on network load when probing a large number of targets

## References

- GitHub: [https://github.com/projectdiscovery/httpx](https://github.com/projectdiscovery/httpx)
