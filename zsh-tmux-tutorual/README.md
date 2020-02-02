## 安装 ZSH

[Youtube Tutorial - Ubuntu 如何安裝 Oh My ZSH](https://youtu.be/lzUGQN-gQE0)

安装 ZSH Shell

```cmd
sudo apt update
sudo apt install zsh
```

查看 zsh 版本

```cmd
zsh --version
```

![alt tag](https://i.imgur.com/jxmkQ3n.png)

找出路徑

```cmd
which zsh
```

![alt tag](https://i.imgur.com/o7u3ZA8.png)

將 zsh 設為當前 使用者 的預設 shell

```cmd
sudo usermod -s /usr/bin/zsh $(whoami)
```

重新打開 linux，在 terminal 輸入 `reboot` 或 `init 6`。

再打開 terminal ，會看到一個視窗，

請選 2，

會自動建立  `~/.zshrc`。

## 安裝 Oh My ZSH

請先安裝 git，

```cmd
sudo apt install git
```

安裝 Oh My ZSH

```cmd
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

更換主題

```cmd
sudo vim ~/.zshrc
```

更改主題 ZSH_THEME="random"，

![alt tag](https://i.imgur.com/motcb96.png)

( 這邊設定為 random，代表每次開都會有不同的主題 )

關閉重開即可生效。

接著來安裝其他的 plugins

## ZSH 主題出現亂碼

有時候安裝主題會出現亂碼, 像是更改主題為 ZSH_THEME="agnoster"

正常應該要顯示

![alt tag](https://i.imgur.com/Wqr3Lbl.png)

可是卻出現了亂碼

![alt tag](https://i.imgur.com/wddi5tk.png)

會出現這個狀況是因為字體沒安裝, 也就是缺少字體,

安裝 Powerline 字體, 請執行以下的指令.

```cmd
sudo apt-get install fonts-powerline
```

更新字體

```cmd
fc-cache -f -v
```

重開 terminal 即可.

### 安装 zsh-autosuggestion

plugins 的路徑可放在兩個地方 ( 則一即可 )，

```text
Standard plugins can be found in ~/.oh-my-zsh/plugins/*
Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
```

先 clone zsh-autosuggestions

```cmd
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/
```

編輯 `.zshrc`，

```cmd
vim ~/.zshrc
````

加入 zsh-autosuggestions，

```.zshrc
plugins=(git zsh-autosuggestions)
```

![alt tag](https://i.imgur.com/TtKcLv6.png)

關掉 Terminal 重開即可，按 ➡ 键 可生效。

效果如下，灰色字體代表生效。

![alt tag](https://i.imgur.com/VHAvlC1.png)

## 安裝 tmux

[Youtube Tutorial - Ubuntu 如何安裝 tmux](https://youtu.be/sKKUgMEOOh0)

tmux 是個 terminal multiplexer，

安裝 tmux，

```cmd
sudo apt-get install tmux
```

tmux 的所有快捷鍵前面都需要加一個 prefix key，預設是 `<Ctrl+b>`。

如果無效，請多加一個 Shift，也就是 `<Ctrl+Shift+b>`。

一些功能說明，

`<Ctrl+b>` + `"` 為 水平分割。

`<Ctrl+b>` + `%` 為 行垂直分割。

更多的快捷鍵功能可參考 [tmux-cheatsheet](https://gist.github.com/MohamedAlaa/2961058)。

一些指令說明

start new
>> tmux

start new with session name:
>> tmux new -s myname

attach (重新開啟 session )
>> tmux a  #  (or at, or attach)

attach to named (重新開啟 session )
>> tmux a -t myname

list sessions:
>> tmux ls

kill session:
>> tmux kill-session -t myname

更改其他 tmux 的設定，

```cmd
sudo vim ~/.tmux.conf
```

修改 `.tmux.conf` 內容

```conf
set -g prefix C-x
set -g mouse on
```

![alt tag](https://i.imgur.com/U9Lfmdy.png)

第一行為修改 prefix 為 `<Ctrl+Shift+x>`。

第二行為可能使用滑鼠控制 tmux。

之後請記得重開機 ( 很重要 !! )，terminal 輸入 `reboot` 或 `init 6`。

之後請將 tmux 加入 Oh My ZSH 的 plugins 中，

可參考[oh-my-zsh/plugins/tmux](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/tmux)。

編輯 `.zshrc`

```cmd
vim ~/.zshrc
````

加入 tmux，

```.zshrc
plugins=(git zsh-autosuggestions tmux)
```

![alt tag](https://i.imgur.com/TtKcLv6.png)

這樣它可以自動記住你的 session。

## 安裝 Tmux Plugin Manager

[Tmux Plugin Manager](https://github.com/tmux-plugins/tpm)

這個我一直裝不起來

當我要用 `prefix + I` 安裝 plugins 時，一直失敗。

## 其他注意事項

在 Oh My ZSH　中，

使用 `git log` 查看 git log 的時候，當輸入 `q` 離開的時候，

log 列表不會保留在畫面上，這時候就又要打一次指令。

可以執行下列指令解決這個問題，

```cmd
git config --global --replace-all core.pager "less -F -X"
```
