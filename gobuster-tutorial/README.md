# gobuster — Web 目錄與子域名暴力列舉工具

gobuster 是以 Go 語言編寫的暴力列舉工具，可用來發現 Web 伺服器上的隱藏目錄/檔案、子域名、虛擬主機等。它透過字典檔（wordlist）逐一嘗試，找出目標上未被公開連結的資源。

## 在滲透流程中的定位

```
subfinder / amass         httpx                  gobuster / nmap
(子域名列舉)        →   (批量探測篩選)      →    (深入掃描)
                                                    │
                                                    ├─ 隱藏的目錄/檔案？
                                                    ├─ 管理後台路徑？
                                                    └─ 其他虛擬主機？
```

當 httpx 篩選出活躍的 Web 目標後，gobuster 負責對這些目標進行深入掃描，找出未被公開的路徑與資源。

## 安裝方式

### Docker 安裝

```bash
docker pull ghcr.io/oj/gobuster:latest
```

### Go 安裝

```bash
go install github.com/OJ/gobuster/v3@latest
```

## 常用模式

| 模式 | 說明 |
|------|------|
| `dir` | 目錄/檔案列舉 — 找出隱藏的路徑、後台、備份檔等 |
| `dns` | 子域名列舉 — 透過字典檔暴力枚舉子域名 |
| `vhost` | 虛擬主機列舉 — 找出同一 IP 上的其他虛擬主機 |

## 字典檔（Wordlist）

gobuster 必須搭配字典檔使用，官方 Docker image 內**不含字典檔**，需要自行準備。

最常用的來源是 **SecLists**：

```bash
git clone https://github.com/danielmiessler/SecLists.git
```

| 用途 | SecLists 路徑 |
|------|--------------|
| 常見目錄/檔案 | `Discovery/Web-Content/common.txt` |
| 中型目錄字典 | `Discovery/Web-Content/directory-list-2.3-medium.txt` |
| 常見子域名 | `Discovery/DNS/subdomains-top1million-5000.txt` |

- SecLists GitHub：[https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)

## 常用指令

### dir 模式 — 目錄/檔案列舉

Docker（需用 `-v` 掛載本機字典檔進容器）：

```bash
docker run --rm \
  -v $(pwd)/SecLists:/wordlists \
  ghcr.io/oj/gobuster dir \
  -u https://example.com \
  -w /wordlists/Discovery/Web-Content/common.txt
```

Go：

```bash
gobuster dir -u https://example.com -w SecLists/Discovery/Web-Content/common.txt
```

輸出範例：

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

每一行結果包含：
- 發現的路徑
- `Status` — HTTP 狀態碼（200 可存取、301 重新導向、403 禁止存取）
- `Size` — 回應內容大小（bytes）
- `-->` — 重新導向的目標位址（如有）

### dns 模式 — 子域名列舉

```bash
gobuster dns -d example.com -w SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

### 常用參數

| 參數 | 說明 |
|------|------|
| `-u <url>` | 目標 URL（dir / vhost 模式） |
| `-w <wordlist>` | 指定字典檔路徑 |
| `-t <n>` | 並發執行緒數（預設 10） |
| `-o <file>` | 輸出結果到檔案 |
| `-x <ext>` | 指定副檔名，例如 `-x php,html,txt` |
| `-s <codes>` | 只顯示指定的狀態碼 |
| `-b <codes>` | 排除指定的狀態碼（預設 404） |
| `-k` | 跳過 TLS 憑證驗證 |
| `-r` | 跟隨重新導向 |
| `-q` | 靜默模式，不顯示 banner |
| `--delay <duration>` | 每次請求之間的延遲，例如 `--delay 100ms` |

### 組合範例

掃描目錄並指定副檔名，輸出到檔案：

```bash
gobuster dir \
  -u https://example.com \
  -w SecLists/Discovery/Web-Content/common.txt \
  -x php,html,bak,txt \
  -t 50 \
  -o results.txt
```

搭配 httpx 篩選後的目標進行批次掃描：

```bash
cat httpx_alive.txt | while read url; do
  gobuster dir -u "$url" -w SecLists/Discovery/Web-Content/common.txt -q
done
```

## 注意事項

- 僅對有授權的目標進行掃描，未經授權對他人系統進行暴力列舉在多數國家屬於違法行為
- 暴力列舉會對目標產生大量請求，可能造成伺服器負載過高，請合理設定 `-t`（執行緒數）與 `--delay`（請求延遲）
- 建議在測試環境中先熟悉工具操作後，再用於正式的授權滲透測試

## 參考連結

- GitHub：[https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
