[中文版](README.md)

# whatweb — Web Technology Fingerprinting

Purpose: identify what technologies a website uses (framework, language, CMS, server)

## Command Breakdown

```bash
whatweb --open-timeout 30 --read-timeout 60 https://example.com
```

| Part | Description |
|------|------|
| `whatweb` | The main program |
| `--open-timeout 30` | Connection timeout; give up if it can't connect within 30 seconds |
| `--read-timeout 60` | Read timeout; give up if the response isn't fully read within 60 seconds |
| `https://example.com` | Scan target |

## How Does It Identify Technologies?

whatweb analyzes multiple clues in the HTTP response:

| Identification basis | Example |
|----------|------|
| HTTP Header | `Server: nginx/1.18.0`, `X-Powered-By: PHP/8.1` |
| HTML content | `<meta name="generator" content="WordPress 6.4">` |
| Cookie name | `PHPSESSID` → PHP, `csrftoken` → Django |
| JavaScript | Includes `react.min.js`, `vue.js`, `jquery.js` |
| HTML structure | Specific class names, DOM structure characteristics |
| URL path | `/wp-admin/` → WordPress, `/admin/` → Django |
| Favicon hash | Default favicons of different frameworks have different hashes |

## Example Output

```
$ whatweb --open-timeout 30 --read-timeout 60 https://example.com

https://example.com [200 OK]
  Country[UNITED STATES],
  HTML5,
  HTTPServer[nginx/1.24.0],
  HttpOnly[session_id],
  IP[93.184.216.34],
  Django,
  OpenSSL,
  Python,
  Script[text/javascript],
  Title[Example Website],
  UncommonHeaders[x-content-type-options],
  X-Frame-Options[SAMEORIGIN]
```

From the output you can tell:

| Info | Description |
|------|------|
| `HTTPServer[nginx/1.24.0]` | The web server is Nginx 1.24.0 |
| `Django` | Uses the Django framework |
| `Python` | The backend language is Python |
| `HttpOnly[session_id]` | The cookie has HttpOnly set (this is good) |
| `X-Frame-Options[SAMEORIGIN]` | Has Clickjacking protection (this is good too) |

## Common Options

| Option | Description |
|------|------|
| `-v` | Verbose output, shows the match details of each plugin |
| `-a 3` | Aggression level (1 lightest ~ 4 heaviest); higher is more accurate but generates more traffic |
| `--log-json=result.json` | Output in JSON format |
| `--no-errors` | Hide error messages |
| `--user-agent` | Custom User-Agent |
| `-i urls.txt` | Batch scan multiple websites |

## Linux Installation

```bash
# Debian / Ubuntu
sudo apt install whatweb

# Or install with gem (whatweb is written in Ruby)
gem install whatweb
```

## Security Implications

- Once the tech stack is identified, attackers can look up CVE vulnerabilities for specific versions
- For example, knowing it's nginx 1.24.0, they can search for known vulnerabilities in that version
- This is a standard tool for the information gathering (Reconnaissance) phase of penetration testing

## Differences from nmap

| Tool | Layer | Focus |
|------|------|------|
| nmap | Network layer | Which ports are open, what services are running |
| whatweb | Application layer | What framework, language, CMS the website uses |

The two complement each other; usually you scan ports with nmap first, then use whatweb to dig deeper into the web services.
