# httpx — 批量 Web 偵察工具

httpx 是 ProjectDiscovery 開發的高速 Web 偵察工具（Go 語言編寫），能批量探測大量 URL，快速取得狀態碼、頁面標題、技術棧、伺服器版本等資訊，幫助在滲透測試中篩選出值得深入調查的目標。

## httpx 能回答的問題

| 資訊類型 | 參數 | 說明 |
|---------|------|------|
| 狀態碼 | `-status-code` | 目標回應的 HTTP 狀態碼（200、301、403、500 等） |
| 頁面標題 | `-title` | 網頁的 `<title>` 標籤內容 |
| 技術偵測 | `-tech-detect` | 使用的框架/CMS（WordPress、React、Nginx 等） |
| 伺服器 | `-web-server` | Web 伺服器軟體與版本 |
| IP 位址 | `-ip` | 解析後的 IP 位址 |
| CDN 偵測 | `-cdn` | 是否使用 CDN（Cloudflare、Akamai 等） |
| 內容長度 | `-content-length` | 回應的 Content-Length |
| TLS 憑證 | `-tls-grab` | TLS 憑證資訊（發行者、到期日等） |

## 核心價值 — 批量處理

與 `curl` 的差別：

```
curl → 一次只能探測一個 URL，需要自己寫迴圈
httpx → 一次可處理上千個目標，自動並發、自動篩選
```

當 subfinder 找出數千個子域名時，httpx 能在幾分鐘內篩出哪些是活的、跑什麼服務、用什麼技術，省去逐一手動檢查的時間。

## 在滲透流程中的定位

```
subfinder / amass         httpx                  gobuster / nmap
(子域名列舉)        →   (批量探測篩選)      →    (深入掃描)
                          │
                          ├─ 哪些子域名是活的？
                          ├─ 跑什麼 Web 服務？
                          └─ 用了哪些技術？
                                    │
                                    ▼
                          挑出高價值目標
                          （如：舊版 CMS、測試環境、管理後台）
```

## 安裝方式

### Docker 安裝

```bash
docker pull projectdiscovery/httpx:latest
```

### Go 安裝

```bash
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

## 常用指令

### 基本用法 — 單一 URL

```bash
echo "https://example.com" | docker run --rm -i projectdiscovery/httpx
```

### 基本用法 — 從檔案讀取

```bash
docker run --rm -i -v $(pwd):/data projectdiscovery/httpx -l /data/urls.txt
```

### 搭配 subfinder 使用（Pipeline）

```bash
docker run --rm -i projectdiscovery/subfinder -d example.com -silent | \
  docker run --rm -i projectdiscovery/httpx -silent
```

### 常用參數

| 參數 | 說明 |
|------|------|
| `-status-code` (`-sc`) | 顯示 HTTP 狀態碼 |
| `-title` | 顯示頁面標題 |
| `-tech-detect` (`-td`) | 偵測使用的技術/框架 |
| `-web-server` | 顯示 Web 伺服器資訊 |
| `-ip` | 顯示解析後的 IP |
| `-cdn` | 偵測是否使用 CDN |
| `-content-length` (`-cl`) | 顯示回應內容長度 |
| `-tls-grab` | 擷取 TLS 憑證資訊 |
| `-o <file>` | 輸出結果到檔案 |
| `-silent` | 靜默模式，只輸出結果 |
| `-threads <n>` | 設定並發數（預設 50） |
| `-follow-redirects` (`-fr`) | 跟隨重新導向 |
| `-mc <code>` | 只顯示符合指定狀態碼的結果 |
| `-fc <code>` | 過濾掉指定狀態碼的結果 |

### 多參數組合範例

一次取得狀態碼、標題、技術棧、伺服器資訊：

```bash
docker run --rm -i -v $(pwd):/data projectdiscovery/httpx \
  -l /data/subdomains.txt \
  -status-code -title -tech-detect -web-server \
  -o /data/httpx_result.txt
```

搭配 subfinder，只列出回應 200 的目標：

```bash
docker run --rm -i projectdiscovery/subfinder -d example.com -silent | \
  docker run --rm -i projectdiscovery/httpx -silent \
  -status-code -title -tech-detect \
  -mc 200
```

## 注意事項

- 僅對有授權的目標進行探測，未經授權掃描他人系統在多數國家屬於違法行為
- httpx 預設並發數為 50，對大量目標探測時注意網路負載

## 參考連結

- GitHub：[https://github.com/projectdiscovery/httpx](https://github.com/projectdiscovery/httpx)
