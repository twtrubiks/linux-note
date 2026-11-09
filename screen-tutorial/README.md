[English Version](README_en.md)

# GNU screen — 退出 SSH 後讓程式繼續在背景執行

`screen` 是一個終端機多工工具（terminal multiplexer），最常見的用途是：

**在遠端 server 上跑程式，退出 SSH 之後讓程式繼續在背景執行**。

功能類似的工具還有 tmux，可參考 [zsh-tmux-tutorual](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual)。

## 運作原理

```
你的電腦 ──SSH──▶ 遠端 server
                    └─ screen session（活在 server 上）
                         └─ 你的程式（在 screen 裡面跑）
```

- **沒用 screen**：SSH 斷線時，shell 會收到 hangup 訊號，底下的程式跟著被關掉。
- **用了 screen**：程式屬於 screen session，跟這次的 SSH 連線無關，detach 之後 `exit` 離開 SSH 也不受影響。

官方手冊說法：

- 程式在 *"the whole screen session is detached from the user's terminal"* 時仍會繼續執行（Section 1 Overview）
- detach 是 *"disconnect it from the terminal and put it into the background"*（Section 8.1 Detach）

### 斷線也不怕：autodetach

手冊 Section 8.1 寫到 *"Autodetach is on by default"*，就算沒按 `Ctrl+A, D`，網路直接斷掉，screen 也會自動 detach，保住裡面的程式。

## 安裝

screen 要安裝在**遠端 server 上**（不是你的電腦），程式也是在 server 上的 screen session 裡面跑。

```cmd
sudo apt install screen
```

## 基本流程

```bash
# 1. SSH 到遠端 server
ssh user@your-server

# 2. 開一個有名字的 screen session
screen -S mywork

# 3. 在裡面跑你的程式
python long_task.py

# 4. 脫離 screen（程式繼續跑）
#    Ctrl+A, D

# 5. 離開 SSH
exit

# --- 之後要接回來 ---
ssh user@your-server
screen -r mywork
```

## Ctrl+A, D 是 detach，不是 exit

這是最容易搞混的地方 :exclamation:

`Ctrl+A, D` 的按法，很多人會按不出來，其實就是兩步：

1. `Ctrl+A` — 同時按住 Ctrl 和 A，然後**放開**
2. `D` — 再**單獨**按 D

| 動作 | 結果 |
|------|------|
| `Ctrl+A, D` | ✅ detach，脫離 screen，程式繼續跑 |
| 在 screen **裡面**直接打 `exit` | ❌ 關掉 shell，該 window 也跟著關掉，程式一起結束 |

手冊 Section 1 Overview 的說法是 *"When a program terminates, screen (per default) kills the window that contained it ... if none are left, screen exits."*，也就是說最後一個 window 的 shell `exit` 之後，整個 screen session 就結束了。

所以**要離開 screen 但讓程式繼續跑，用 `Ctrl+A, D`，不要用 `exit`**。

## 常用指令

| 指令 | 說明 |
|------|------|
| `screen -S <name>` | 開一個新的 session 並命名 |
| `screen -ls` | 列出所有 session |
| `screen -r <name>` | 接回一個已 detach 的 session |
| `screen -d -r <name>` | 如果 session 還掛在別的地方，先把它 detach 再接回來 |
| `screen -x <name>` | 接上一個已經在別處 attach 的 session（多個終端同時看同一個畫面） |
| `screen -X -S <name> quit` | 從外部直接終止某個 session（不用 attach 進去） |

### screen -ls 顯示 Attached 怎麼辦

手冊 Section 3 對 `-ls` 的說明：標示 detached 的 session 可以用 `screen -r` 接回，標示 attached 的代表還有終端連著。

而 `-r` 是 *"Resume a detached screen session"*，所以當 session 顯示 `(Attached)`（例如舊的 SSH 連線還沒 timeout），`screen -r` 會接不回去，這時改用

```cmd
screen -d -r <name>
```

`-d -r` 的手冊說明是 *"Reattach a session and if necessary detach it first"*。

### 刪除 session

刪除特定的 session

```bash
# 方法一：先 attach 進去再 exit
screen -r mywork
exit

# 方法二：直接從外部終止（不需要 attach）
screen -X -S mywork quit
```

刪除全部 session

```bash
screen -ls | grep -oP '\d+\.\S+' | xargs -I {} screen -X -S {} quit
```

## screen 內的快捷鍵

所有快捷鍵都是先按 `Ctrl+A`，放開，再按下一個鍵。

| 快捷鍵 | 說明 |
|--------|------|
| `Ctrl+A, D` | **detach**，脫離 screen，程式繼續跑 |
| `Ctrl+A, C` | 開一個新的 window（新 shell） |
| `Ctrl+A, N` / `Ctrl+A, P` | 切到下一個 / 上一個 window |
| `Ctrl+A, 0~9` | 切到指定編號的 window |
| `Ctrl+A, "` | 列出所有 window 讓你選 |
| `Ctrl+A, Shift+A` | 幫目前的 window 命名 |
| `Ctrl+A, K` | 關掉目前的 window |
| `Ctrl+A, [` | 進入捲動 / 複製模式（可往回看輸出） |
| `Ctrl+A, ?` | 顯示所有快捷鍵 |
| `Ctrl+A, A` | 送出一個真正的 `Ctrl+A` 給程式（例如 bash 的「游標移到行首」） |

捲動模式要離開，按 `Esc` 即可，手冊 Section 12.1.8 Specials 寫到 *"All keys not described here exit copy mode."*，`Esc` 不在捲動模式的按鍵清單中，所以會直接離開。

注意 :exclamation: screen 的 prefix key 是 `Ctrl+A`，剛好和 bash / zsh 的「游標移到行首」衝突，在 screen 裡面要移到行首請改按 `Ctrl+A, A`。

tmux 的 prefix key 則是 `Ctrl+B`，所以不會有這個問題。

## 注意事項

| 情況 | 結果 |
|------|------|
| `Ctrl+A, D` 之後 `exit` 離開 SSH | ✅ 程式繼續跑 |
| SSH 突然斷線 | ✅ 程式繼續跑（autodetach） |
| 在 screen **裡面**直接打 `exit` | ❌ 整個 screen session（或該 window）會關掉，程式也一起結束 |
| 遠端 server 重開機 | ❌ screen session 會消失，要重新啟動 |

server 重開機那一列是推論，不是手冊原文：screen session 本身就是 server 上的一般程式，重開機一定會被結束。

## 實際應用 - Claude Code Remote Control

在遠端 server 用 screen 跑 Claude Code 的 remote control，退出 SSH 後用手機繼續操控。

```bash
# Step 1: SSH 到遠端 server
ssh user@your-server

# Step 2: 開一個 screen session
screen -S claude

# Step 3: 啟動 claude code 的 remote control
claude remote-control

# Step 4: 手機連上（方式請參考官方文件）

# Step 5: 安全脫離 screen（不會關掉 Claude）
#         Ctrl+A, D

# Step 6: 退出 SSH
exit
```

之後要從電腦接回來

```bash
ssh user@your-server
screen -r claude
```

Remote Control 的細節可參考 [Claude Code Remote Control 官方文件](https://code.claude.com/docs/en/remote-control)。

## 參考連結

- [GNU Screen 官方手冊](https://www.gnu.org/software/screen/manual/screen.html)
  - Section 1 Overview：detach 後程式繼續執行、程式結束會關掉 window
  - Section 3 Invoking Screen：`-S`、`-r`、`-ls`、`-x`、`-d -r`、`-X` 參數
  - Section 5.1 Default Key Bindings：快捷鍵
  - Section 8.1 Detach：detach 與 autodetach
  - Section 12.1 Copying：捲動 / 複製模式
- [Debian screen 套件](https://packages.debian.org/search?keywords=screen&exact=1)
