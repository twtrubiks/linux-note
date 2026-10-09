[中文版](README.md)

# nmap — Network Port Scanner

Purpose: scan a target host to find which ports are open and what services are running

## Common Options

| Option | Description |
|------|------|
| `-sV` | Detect service versions (Service Version Detection) |
| `-sC` | Run the default NSE scripts (same as `--script=default`) to check for common vulnerabilities |
| `-sS` | SYN scan (half-open scan, stealthier, requires root) |
| `-sT` | TCP connect (full connection) scan |
| `-sU` | UDP port scan (requires root) |
| `-p` | Specify a port range, e.g. `-p 1-1000` or `-p 22,80,443` |
| `-p-` | Scan all 65535 ports |
| `-O` | Detect the operating system (requires root) |
| `-A` | Aggressive scan (includes OS detection, version detection, scripts, traceroute; requires root) |
| `--top-ports` | Only scan the N most common ports, e.g. `--top-ports 100` |
| `-T4` | Speed up the scan (T0 slowest ~ T5 fastest) |
| `-n` | Skip DNS resolution (faster) |
| `-oN` | Write results to a text file |
| `-oA` | Output in all three formats at once (.nmap / .xml / .gnmap) |

## Command Breakdown: `nmap -sV -sC example.com`

```
nmap -sV -sC example.com
```

### `-sV`: Detect Service Versions

Sends probe packets to open ports and determines the software and version based on the responses.

```
# Without -sV → you only know the port is open
80/tcp open http

# With -sV → you know exactly which software and version it is
80/tcp open http nginx 1.18.0
```

Once you know the version, you can look up the corresponding CVE vulnerabilities.

### `-sC`: Run the Default Scripts

Same as `--script=default`; runs nmap's built-in NSE scripts (Nmap Scripting Engine).

| Service | What the scripts do |
|------|-------------|
| SSH | Get the host key and supported algorithms |
| HTTP | Grab the page title, check robots.txt, detect common paths |
| SMB | List shared folders, detect vulnerabilities such as EternalBlue |
| MySQL | Try anonymous login, get database information |
| FTP | Check whether anonymous login is allowed |

### Execution Flow

```
1. DNS resolve example.com → get the IP
2. Host discovery → check whether the target is alive
3. Port scan → by default scans the 1000 most common ports
4. -sV kicks in → probe the service version on each open port
5. -sC kicks in → run the default scripts on each service for further checks
6. Output the results
```

## Example Output

```
Starting Nmap 7.94 ( https://nmap.org )
Nmap scan report for example.com (93.184.216.34)

PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.9 (protocol 2.0)
| ssh-hostkey:                          ← extra info produced by the -sC scripts
|   256 aa:bb:cc:dd (ECDSA)
|   256 ee:ff:00:11 (ED25519)
80/tcp   open  http     nginx 1.18.0
|_http-title: Welcome Page              ← -sC automatically grabbed the page title
|_http-server-header: nginx/1.18.0
443/tcp  open  https    nginx 1.18.0
| ssl-cert: Subject: CN=example.com     ← -sC automatically checked the SSL certificate
3306/tcp open  mysql    MySQL 8.0.32
| mysql-info:                           ← -sC automatically probed MySQL info
|   Protocol: 10
|   Version: 8.0.32
```

Lines starting with `|` are the extra results from the `-sC` scripts.

## Security Implications

- **3306/tcp (MySQL) exposed to the outside**: a database should not be directly exposed to the public internet; attackers can try brute-forcing it or exploiting known vulnerabilities
- **8080/tcp (Node.js Express)**: could be a development environment, an admin panel, or an unprotected API; it should not appear in production
- **Version information exposed**: once attackers know it's nginx 1.18.0 and MySQL 8.0.32, they can look up the corresponding CVE vulnerabilities

## Linux Installation

```bash
# Debian / Ubuntu
sudo apt update && sudo apt install nmap

# CentOS / RHEL / Fedora
sudo dnf install nmap

# Arch Linux
sudo pacman -S nmap
```

## Practical Examples

```bash
# Basic service version scan + default scripts
nmap -sV -sC example.com

# Aggressive scan
sudo nmap -A example.com

# Only scan specific ports
nmap -p 22,80,443,3306,8080 example.com

# Scan all hosts on the LAN (only check which are alive)
nmap -sn 192.168.1.0/24

# Scan all ports + fast mode
nmap -p- -T4 example.com

# Write results to a file
nmap -sV -oN result.txt example.com

# Scan localhost (safe practice)
nmap -p- localhost
```

## Notes

- Only scan targets you are authorized to scan; scanning other people's systems without authorization is illegal in most countries
- Scan types that require root privileges (`-sS`, `-O`, `-A`, `-sU`) need `sudo`
- Without `sudo`, nmap automatically falls back to a TCP connect scan (`-sT`)
