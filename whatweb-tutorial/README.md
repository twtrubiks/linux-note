# whatweb — Web 技術指紋識別

目的：識別網站用了什麼技術（框架、語言、CMS、伺服器）

## 指令解析

```bash
whatweb --open-timeout 30 --read-timeout 60 https://example.com
```

| 部分 | 說明 |
|------|------|
| `whatweb` | 主程式 |
| `--open-timeout 30` | 連線建立的超時時間，30 秒內連不上就放棄 |
| `--read-timeout 60` | 讀取回應的超時時間，60 秒內沒讀完就放棄 |
| `https://example.com` | 掃描目標 |

## 它怎麼辨識技術的？

whatweb 會分析 HTTP 回應中的多種線索：

| 辨識依據 | 範例 |
|----------|------|
| HTTP Header | `Server: nginx/1.18.0`、`X-Powered-By: PHP/8.1` |
| HTML 內容 | `<meta name="generator" content="WordPress 6.4">` |
| Cookie 名稱 | `PHPSESSID` → PHP、`csrftoken` → Django |
| JavaScript | 引入了 `react.min.js`、`vue.js`、`jquery.js` |
| HTML 結構 | 特定 class 名稱、DOM 結構特徵 |
| URL 路徑 | `/wp-admin/` → WordPress、`/admin/` → Django |
| Favicon hash | 不同框架預設的 favicon 有不同的 hash |

## 範例輸出

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

從輸出可以讀出：

| 資訊 | 說明 |
|------|------|
| `HTTPServer[nginx/1.24.0]` | Web Server 是 Nginx 1.24.0 |
| `Django` | 使用 Django 框架 |
| `Python` | 後端語言是 Python |
| `HttpOnly[session_id]` | Cookie 有設 HttpOnly（這是好的） |
| `X-Frame-Options[SAMEORIGIN]` | 有防 Clickjacking（這也是好的） |

## 常用參數

| 參數 | 說明 |
|------|------|
| `-v` | 詳細輸出，顯示每個插件的匹配細節 |
| `-a 3` | 侵略等級（1 最輕 ~ 4 最重），越高越準但流量越大 |
| `--log-json=result.json` | 輸出 JSON 格式 |
| `--no-errors` | 隱藏錯誤訊息 |
| `--user-agent` | 自訂 User-Agent |
| `-i urls.txt` | 批次掃描多個網站 |

## Linux 安裝

```bash
# Debian / Ubuntu
sudo apt install whatweb

# 或用 gem 安裝（whatweb 是 Ruby 寫的）
gem install whatweb
```

## 資安意義

- 辨識出技術棧後，攻擊者可以針對特定版本查 CVE 漏洞
- 例如知道是 nginx 1.24.0，就能搜尋該版本的已知漏洞
- 這是滲透測試資訊蒐集（Reconnaissance）階段的標準工具

## 和 nmap 的差異

| 工具 | 層級 | 重點 |
|------|------|------|
| nmap | 網路層 | 開了哪些端口、跑什麼服務 |
| whatweb | 應用層 | 網站用了什麼框架、語言、CMS |

兩者互補，通常先用 nmap 掃端口，再用 whatweb 深入分析 Web 服務。
