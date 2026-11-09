[中文版](README.md)

# Amass — OWASP Subdomain Enumeration Tool

Amass is an open-source tool maintained by OWASP for in-depth subdomain enumeration and asset inventory.

It combines multiple techniques (passive intelligence, active probing, DNS brute forcing, certificate transparency logs, etc.) and can discover more subdomains than typical tools, making it well suited for attack surface reconnaissance in the early stages of a penetration test.

## Subfinder vs Amass Comparison

| Item | Subfinder | Amass |
|---------|-----------|-------|
| Speed | Fast (seconds to minutes) | Slow (minutes to hours) |
| Amount of data | Less | More (usually 20-50% more) |
| Method | Mainly passive intelligence (API queries) | Passive + active (DNS brute forcing, network range scanning, certificate transparency) |
| Resource usage | Low | High (CPU, memory, network) |
| Use cases | Quick recon, CI/CD automation | In-depth asset inventory, full attack surface analysis |

## Why Amass Finds More but Is Slower

- **Multiple enumeration techniques**: Besides querying APIs, it also does DNS brute forcing, Zone Transfer attempts, NSEC Walking, etc.
- **Recursive discovery**: After finding a subdomain, it runs further enumeration on it, digging deeper layer by layer
- **ASN / network range mapping**: It looks up the target's ASN and scans reverse DNS across the entire network range to find more related domains
- **Certificate transparency logs**: It crawls all related certificates in CT logs and extracts subdomains from them

## Practical Recommendations

Recommended workflow:

```
1. Quick scan with Subfinder first → get initial results within seconds
2. Then deep scan with Amass → take the time to find more hidden subdomains
3. Merge and deduplicate → combine both results and remove duplicates
```

Commands to merge and deduplicate:

```bash
# Output each to a separate file
subfinder -d example.com -o subfinder_results.txt
# amass (see below for the docker version)
# Merge and deduplicate
cat subfinder_results.txt amass_results.txt | sort -u > all_subdomains.txt
```

## Installation (Docker)

```bash
docker pull owaspamass/amass:latest
```

## Common Commands

### Basic Subdomain Enumeration

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -d example.com
```

Parameter description:

| Parameter | Description |
|------|------|
| `--rm` | Automatically remove the container when it exits |
| `-it` | Interactive mode, so you can see real-time output |
| `-v ~/amass:/.config/amass` | Mount a local directory to keep config and results |
| `enum` | Run subdomain enumeration |
| `-d` | Specify the target domain |

### Passive Mode Only (No Requests Sent Directly to the Target)

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -passive -d example.com
```

### Output to a File

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -d example.com -o amass_results.txt
```

## Querying Results

Amass stores results in a SQLite database, which you can query as follows:

```bash
sqlite3 ~/amass/assetdb.db
```

```sql
-- List all tables
.tables

-- Query discovered assets
SELECT * FROM assets LIMIT 10;
```

## Notes

- Only scan targets you are authorized to scan; scanning other people's systems without authorization is illegal in most countries
- Amass's active mode sends a large number of DNS requests to the target, which may trigger firewall or IDS alerts
- If you only want passive reconnaissance, use the `-passive` parameter

## References

- Official docs: [https://owasp-amass.github.io/docs/](https://owasp-amass.github.io/docs/)
- GitHub: [https://github.com/owasp-amass/amass](https://github.com/owasp-amass/amass)
