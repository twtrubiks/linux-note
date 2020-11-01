# arandr - 設定多螢幕工具

[Youtube Tutorial - 設定多螢幕工具 - arandr](https://youtu.be/PVMRLJaTNSY)

很多時候在使用 linux 時, 當有使用多螢幕, 有時候可能插拔了一下,

就會發現設定跑掉了, 每次都要重新設定很麻煩:sweat:

所以今天我們就來透過 arandr 快速解決這個問題:smile:

## 安裝 arandr

```shell
sudo apt install arandr
```

打開軟體會像這樣, 先排好你需要的螢幕

![alt tag](https://i.imgur.com/GmlIBsQ.png)

接著保存你的設定檔案, 路徑預設會在 `~/.screenlayout/` 裡面.

例如我的檔名叫作 `work.sh`,

我只需要再搭配之前教大家的 alias, 文章可參考 [善用 alias](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual#%E5%96%84%E7%94%A8-alias),

[Youtube Tutorial - Linux 教學 - ZSH alias](https://youtu.be/ei2Bp2gu1OM)

舉例, `alias work="sh /home/twtrubiks/.screenlayout/work.sh"`

就可以透過一行指令 `work` 設定好螢幕的排版.
