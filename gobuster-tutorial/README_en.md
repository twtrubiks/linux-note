[中文版](README.md)

# gobuster — Web Directory and Subdomain Brute-Force Enumeration Tool

gobuster is a brute-force enumeration tool written in Go. It can be used to discover hidden directories/files, subdomains, virtual hosts, etc. on web servers. It works through a wordlist entry by entry to find resources on the target that aren't publicly linked.

## Where It Fits in the Pentest Workflow

```
subfinder / amass         httpx                  gobuster / nmap
(subdomain enum)    →   (probe & filter)    →    (deep scan)
                                                    │
                                                    ├─ Hidden directories/files?
                                                    ├─ Admin panel paths?
                                                    └─ Other virtual hosts?
```

Once httpx has filtered out the live web targets, gobuster takes care of scanning these targets in depth to find paths and resources that aren't public.

## Installation

### Docker Installation

```bash
docker pull ghcr.io/oj/gobuster:latest
```

### Go Installation

```bash
go install github.com/OJ/gobuster/v3@latest
```

## Common Modes

| Mode | Description |
|------|------|
| `dir` | Directory/file enumeration — find hidden paths, admin panels, backup files, etc. |
| `dns` | Subdomain enumeration — brute-force subdomains using a wordlist |
| `vhost` | Virtual host enumeration — find other virtual hosts on the same IP |

## Wordlist

gobuster must be used with a wordlist. The official Docker image **does not include any wordlists**, so you need to prepare them yourself.

The most commonly used source is **SecLists**:

```bash
git clone https://github.com/danielmiessler/SecLists.git
```

| Purpose | SecLists Path |
|------|--------------|
| Common directories/files | `Discovery/Web-Content/common.txt` |
| Medium-sized directory wordlist | `Discovery/Web-Content/directory-list-2.3-medium.txt` |
| Common subdomains | `Discovery/DNS/subdomains-top1million-5000.txt` |

- SecLists GitHub: [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)

## Common Commands

### dir Mode — Directory/File Enumeration

Docker (you need `-v` to mount the local wordlist into the container):

```bash
docker run --rm \
  -v $(pwd)/SecLists:/wordlists \
  ghcr.io/oj/gobuster dir \
  -u https://example.com \
  -w /wordlists/Discovery/Web-Content/common.txt
```

Go:

```bash
gobuster dir -u https://example.com -w SecLists/Discovery/Web-Content/common.txt
```

Sample output:

```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://example.com
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /wordlists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 185] [--> https://example.com/admin/]
/api                  (Status: 200) [Size: 1053]
/backup               (Status: 403) [Size: 162]
/config.php           (Status: 200) [Size: 512]
/login                (Status: 200) [Size: 3847]
/uploads              (Status: 301) [Size: 185] [--> https://example.com/uploads/]
Progress: 4727 / 4727 (100.00%)
===============================================================
Finished
===============================================================
```

Each result line contains:
- The discovered path
- `Status` — HTTP status code (200 accessible, 301 redirect, 403 forbidden)
- `Size` — response body size (bytes)
- `-->` — redirect target (if any)

### dns Mode — Subdomain Enumeration

```bash
gobuster dns -d example.com -w SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

### Common Options

| Option | Description |
|------|------|
| `-u <url>` | Target URL (dir / vhost mode) |
| `-w <wordlist>` | Path to the wordlist |
| `-t <n>` | Number of concurrent threads (default 10) |
| `-o <file>` | Write results to a file |
| `-x <ext>` | File extensions to try, e.g. `-x php,html,txt` |
| `-s <codes>` | Only show the specified status codes |
| `-b <codes>` | Exclude the specified status codes (default 404) |
| `-k` | Skip TLS certificate verification |
| `-r` | Follow redirects |
| `-q` | Quiet mode, don't show the banner |
| `--delay <duration>` | Delay between each request, e.g. `--delay 100ms` |

### Combined Examples

Scan directories with specific file extensions and write the output to a file:

```bash
gobuster dir \
  -u https://example.com \
  -w SecLists/Discovery/Web-Content/common.txt \
  -x php,html,bak,txt \
  -t 50 \
  -o results.txt
```

Batch scan the targets filtered by httpx:

```bash
cat httpx_alive.txt | while read url; do
  gobuster dir -u "$url" -w SecLists/Discovery/Web-Content/common.txt -q
done
```

## Notes

- Only scan targets you are authorized to test. Brute-force enumeration against other people's systems without authorization is illegal in most countries
- Brute-force enumeration sends a large number of requests to the target and may overload the server, so set `-t` (thread count) and `--delay` (request delay) reasonably
- It's recommended to get familiar with the tool in a test environment first, before using it in a real authorized penetration test

## References

- GitHub: [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
