# nmap — 網路端口掃描器

目的：掃描目標主機開了哪些端口、跑了什麼服務

## 常用參數

| 參數 | 說明 |
|------|------|
| `-sV` | 偵測服務版本（Service Version Detection） |
| `-sC` | 執行預設 NSE 腳本（等同 `--script=default`），檢查常見漏洞 |
| `-sS` | SYN 掃描（半開放掃描，較隱蔽，需要 root） |
| `-sT` | TCP 全連接掃描 |
| `-sU` | UDP 端口掃描（需要 root） |
| `-p` | 指定端口範圍，例如 `-p 1-1000` 或 `-p 22,80,443` |
| `-p-` | 掃描所有 65535 個端口 |
| `-O` | 偵測作業系統（需要 root） |
| `-A` | 全面掃描（含 OS 偵測、版本偵測、腳本、traceroute，需要 root） |
| `--top-ports` | 只掃描最常見的 N 個端口，例如 `--top-ports 100` |
| `-T4` | 加快掃描速度（T0 最慢 ~ T5 最快） |
| `-n` | 跳過 DNS 解析（加速） |
| `-oN` | 輸出結果到文字檔 |
| `-oA` | 同時輸出三種格式（.nmap / .xml / .gnmap） |

## 指令解析：`nmap -sV -sC example.com`

```
nmap -sV -sC example.com
```

### `-sV`：偵測服務版本

對開放端口送出探測封包，根據回應判斷軟體和版本。

```
# 沒有 -sV → 只知道端口開著
80/tcp open http

# 有 -sV → 知道具體是什麼軟體和版本
80/tcp open http nginx 1.18.0
```

知道版本後就能查對應的 CVE 漏洞。

### `-sC`：跑預設腳本

等同 `--script=default`，執行 nmap 內建的 NSE 腳本（Nmap Scripting Engine）。

| 服務 | 腳本會做什麼 |
|------|-------------|
| SSH | 取得 host key、支援的演算法 |
| HTTP | 抓取網頁標題、檢查 robots.txt、偵測常見路徑 |
| SMB | 列出共享資料夾、偵測 EternalBlue 等漏洞 |
| MySQL | 嘗試匿名登入、取得資料庫資訊 |
| FTP | 檢查是否允許匿名登入 |

### 執行流程

```
1. DNS 解析 example.com → 取得 IP
2. 主機發現 → 確認目標是否存活
3. 端口掃描 → 預設掃描最常見的 1000 個端口
4. -sV 生效 → 對每個 open 端口探測服務版本
5. -sC 生效 → 對每個服務跑預設腳本做進一步檢查
6. 輸出結果
```

## 範例輸出

```
Starting Nmap 7.94 ( https://nmap.org )
Nmap scan report for example.com (93.184.216.34)

PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.9 (protocol 2.0)
| ssh-hostkey:                          ← -sC 腳本產生的額外資訊
|   256 aa:bb:cc:dd (ECDSA)
|   256 ee:ff:00:11 (ED25519)
80/tcp   open  http     nginx 1.18.0
|_http-title: Welcome Page              ← -sC 自動抓了網頁標題
|_http-server-header: nginx/1.18.0
443/tcp  open  https    nginx 1.18.0
| ssl-cert: Subject: CN=example.com     ← -sC 自動檢查了 SSL 憑證
3306/tcp open  mysql    MySQL 8.0.32
| mysql-info:                           ← -sC 自動探測了 MySQL 資訊
|   Protocol: 10
|   Version: 8.0.32
```

有 `|` 開頭的行，就是 `-sC` 腳本額外跑出來的結果。

## 資安意義

- **3306/tcp (MySQL) 對外開放**：資料庫不應直接暴露在公網，攻擊者可嘗試暴力破解或利用已知漏洞
- **8080/tcp (Node.js Express)**：可能是開發環境、管理後台、或未經保護的 API，不應出現在正式環境
- **版本資訊暴露**：知道 nginx 1.18.0、MySQL 8.0.32 後，攻擊者可查找對應的 CVE 漏洞

## Linux 安裝

```bash
# Debian / Ubuntu
sudo apt update && sudo apt install nmap

# CentOS / RHEL / Fedora
sudo dnf install nmap

# Arch Linux
sudo pacman -S nmap
```

## 實用範例

```bash
# 基本服務版本掃描 + 預設腳本
nmap -sV -sC example.com

# 全面掃描
sudo nmap -A example.com

# 只掃描特定端口
nmap -p 22,80,443,3306,8080 example.com

# 掃描區網所有主機（僅探測存活）
nmap -sn 192.168.1.0/24

# 掃描所有端口 + 快速模式
nmap -p- -T4 example.com

# 輸出結果到檔案
nmap -sV -oN result.txt example.com

# 掃描本機（安全練習）
nmap -p- localhost
```

## 注意事項

- 僅對有授權的目標進行掃描，未經授權掃描他人系統在多數國家屬於違法行為
- 需要 root 權限的掃描方式（`-sS`、`-O`、`-A`、`-sU`）需加 `sudo`
- 不加 `sudo` 時，nmap 會自動降級為 TCP connect 掃描（`-sT`）
