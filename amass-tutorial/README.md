# Amass — OWASP 子域名列舉工具

Amass 是 OWASP 維護的開源工具，用於深度子域名列舉與資產盤點。

它整合了多種技術（被動情報、主動探測、DNS 暴力破解、憑證透明度日誌等），能發現比一般工具更多的子域名，適合滲透測試前期的攻擊面偵察。

## Subfinder vs Amass 比較

| 比較項目 | Subfinder | Amass |
|---------|-----------|-------|
| 速度 | 快（幾秒到幾分鐘） | 慢（幾分鐘到幾小時） |
| 資料量 | 較少 | 較多（通常多 20-50%） |
| 方法 | 被動情報為主（API 查詢） | 被動 + 主動（DNS 暴力、網段掃描、憑證透明度） |
| 資源消耗 | 低 | 高（CPU、記憶體、網路） |
| 適用場景 | 快速偵察、CI/CD 自動化 | 深度資產盤點、完整攻擊面分析 |

## 為什麼 Amass 資料多但慢

- **多種列舉技術**：除了查 API，還會做 DNS 暴力破解、Zone Transfer 嘗試、NSEC Walking 等
- **遞迴發現**：找到一個子域名後，會對它再做進一步列舉，層層挖掘
- **ASN / 網段映射**：會查出目標的 ASN，掃描整個網段的反向 DNS，找出更多關聯域名
- **憑證透明度日誌**：爬取 CT logs 中的所有相關憑證，從中提取子域名

## 實務建議

建議的工作流程：

```
1. 先用 Subfinder 快速掃 → 幾秒內取得初步結果
2. 再用 Amass 深度掃 → 花時間找出更多隱藏子域名
3. 合併去重 → 將兩者結果合併，移除重複項
```

合併去重的指令：

```bash
# 分別輸出到檔案
subfinder -d example.com -o subfinder_results.txt
# amass (docker 版本見下方)
# 合併去重
cat subfinder_results.txt amass_results.txt | sort -u > all_subdomains.txt
```

## 安裝方式（Docker）

```bash
docker pull owaspamass/amass:latest
```

## 常用指令

### 基本子域名列舉

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -d example.com
```

參數說明：

| 參數 | 說明 |
|------|------|
| `--rm` | 容器結束後自動移除 |
| `-it` | 互動模式，可看到即時輸出 |
| `-v ~/amass:/.config/amass` | 掛載本機目錄，保存設定與結果 |
| `enum` | 執行子域名列舉 |
| `-d` | 指定目標域名 |

### 僅被動模式（不主動發送任何請求到目標）

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -passive -d example.com
```

### 輸出到檔案

```bash
docker run --rm -it -v ~/amass:/.config/amass owaspamass/amass enum -d example.com -o amass_results.txt
```

## 查詢結果

Amass 會將結果存入 SQLite 資料庫，可用以下方式查詢：

```bash
sqlite3 ~/amass/assetdb.db
```

```sql
-- 查看所有資料表
.tables

-- 查詢已發現的資產
SELECT * FROM assets LIMIT 10;
```

## 注意事項

- 僅對有授權的目標進行掃描，未經授權掃描他人系統在多數國家屬於違法行為
- Amass 的主動模式會對目標發送大量 DNS 請求，可能觸發防火牆或 IDS 警報
- 如果只想做被動偵察，使用 `-passive` 參數

## 參考連結

- 官方文件：[https://owasp-amass.github.io/docs/](https://owasp-amass.github.io/docs/)
- GitHub：[https://github.com/owasp-amass/amass](https://github.com/owasp-amass/amass)
