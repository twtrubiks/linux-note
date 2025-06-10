# Whisper 本地 YouTube 字幕生成指南 (無需Key)：從新手入門到 AI 校正

[English Version](README_en.md)

* [Youtube Tutorial - Whisper YouTube 中文字幕生成指南 (無需Key)：從新手入門到 AI 校正](https://youtu.be/E-X3kp8wCIg)

上 youtube 中文字幕就靠它 [whisper](https://github.com/openai/whisper)

這邊會強調中文, 不是它只能辨識中文, 英文當然也可以, 是因為中文表現的也很好.

Whisper 不需要輸入任何的 AI KEY, 因為他是直接透過下載的 model 進行本地辨識.

## 安裝

```cmd
pip install -U openai-whisper
```

處理影片通常需要安裝 ffmpeg(用於影音格式處理), 請再執行 (假設你是 Debian/Ubuntu)

```cmd
sudo apt update && sudo apt install ffmpeg
```

## 教學

如果你是一般使用者, 目標是會議, 影片字幕, 使用 `medium` model 即可.

Whisper 提供多種模型，如 tiny, base, small, large，medium 在速度和準確度之間取得良好平衡.

若追求最高準確度可嘗試 large，但需要更多運算資源；若資源有限或追求速度則可選用較小的模型.

使用方法

```cmd
whisper test.mkv --language Chinese --model medium --output_format srt
```

第一次執行會比較久, 因為會去下載 `medium` 這個 model.

這邊我們只產出 srt 字幕檔, 不只影片檔案, 音訊檔案如 `.mp3` 這些也都是沒問題的

這邊補充一下,

如果你沒顯卡, 推薦使用 [Google Colab](https://colab.google/)

我使用 T4 GPU (免費, 但一天有使用量限制)

![alt tag](https://cdn.imgpile.com/f/kEJuw8y_xl.png)

`!` 告訴 Colab, 接下來的這行不是 Python 程式碼，請把它當作在終端機裡輸入的指令來執行

![alt tag](https://cdn.imgpile.com/f/eurDpGD_xl.png)

大約10分鐘的影片, `CPU` 花了35分鐘, `GPU` 花了3分鐘.

我自己使用上覺得 `medium` 這個 model 比較完美, 因為我發現 `large-v3` 在中文斷句上沒有很好.

## 流程, Whisper 初稿 -> AI + Prompt 校正專有名詞

然後我會再把產出的 srt 字幕檔透過 AI 下 Prompt 去校正我的字幕檔 (我使用 Gemini 2.5 Pro),

我通常會這樣給它

```text
請依據 srt 格式修正以下文字，不要更動時間戳或格式結構.

幫我修正專有名詞

這是一篇 參考

Dify 安裝與連接 Ollama 設定指南 (Docker Compose)  的介紹 RAG

https://github.com/twtrubiks/dify-ollama-docker-tutorial/blob/main/dify.md

<這邊我會再把整個 srt 字幕檔貼上去>
```

我發現透過這樣的方式, 字幕檔正確性很高(有9成以上).

## 和 Google AI Studio 比較

我也有透過 Google AI Studio 直接貼給它 Prompt, 類似如下

```text
你是一個專業的 產生 srt 字幕檔 員工,

格式範例為

時間戳行 (00:00:00,000 --> 00:00:02,500)

這是 SRT 檔案中最關鍵的部分之一，它精確地定義了對應字幕文字的顯示時機。

格式: 時:分:秒,毫秒 --> 時:分:秒,毫秒

HH:MM:SS,ms
HH: 小時 (00-99)
MM: 分鐘 (00-59)
SS: 秒 (00-59)
ms: 毫秒 (000-999) - 注意： SRT 標準使用逗號 (,) 作為秒和毫秒之間的分隔符。
-->: 分隔符，表示從「開始時間」到「結束時間」。
00:00:00,000 (開始時間):

這表示對應的字幕文字應該在影片播放到 0 小時 0 分鐘 0 秒又 0 毫秒 這個精確的時間點開始顯示在畫面上。
00:00:02,500 (結束時間):

這表示對應的字幕文字應該在影片播放到 0 小時 0 分鐘 2 秒又 500 毫秒 這個精確的時間點從畫面上消失。

- 如果段落過長, 字幕不要超過三秒.
```

但我發現這種錯誤率很高, 有時候不是 srt 格式錯誤, 就是但後面的時候一口氣字幕段落超長,

畢竟專業的事情還是給 whisper 做比較靠普.

## 產生特定區間字幕檔

如果你只想產生特定區間的字幕, 可以透過 `ffmpeg` 進行切割

```cmd
ffmpeg -i test.mkv -ss 5 -to 8 -c copy clipped_test.mkv
```

-ss 5：起始時間, 從影片第 5 秒開始（5秒 = 0:05）

-to 8：結束時間, 到影片第 8 秒結束（8秒 = 0:08）
