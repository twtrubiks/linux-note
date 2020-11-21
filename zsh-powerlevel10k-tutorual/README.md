## zsh 搭配 powerlevel10k

[Youtube Tutorial - zsh-powerlevel10k 教學 - 漂亮的 terminal](https://youtu.be/5f8w7gYnnfU)

官方文件 [https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

先來上一張圖

![alt tag](https://i.imgur.com/kdxyTtT.png)

這邊範例使用 Oh My Zsh, 如果你還沒安裝, 請參考之前的文章

[如何安裝 Oh My ZSH](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual)


## 教學

在開始安裝之前, 強烈建議先安裝字體 (不然有些圖可能會顯示不出來:sweat:)

安裝流程 [https://github.com/romkatv/powerlevel10k#get-started](https://github.com/romkatv/powerlevel10k#get-started)

## 安裝字體

[the recommended font](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)

[how-was-the-recommended-font-created](https://github.com/romkatv/powerlevel10k#how-was-the-recommended-font-created)

自己 build 出字體 ( 使用 docker build, 我自己 clone 加上 build 是還蠻久的:sweat_smile:)

或是直接下載現成的 [Manual font installation](https://github.com/romkatv/powerlevel10k#manual-font-installation)

官方推薦使用 MesloLGS NF font

```cmd
git clone --depth=1 https://github.com/romkatv/nerd-fonts.git
cd nerd-fonts
./build 'Meslo/S/*'
```

如果一切沒問題, ttf 檔案會出現在 `./out`.

![alt tag](https://i.imgur.com/puW0EkH.png)

安裝字體的方式,

將你的字體 copy 到 `/usr/share/fonts/truetype/` 裡面,

然後執行 `sudo fc-cache -f -v` (更新字體快取).

設定 vscode terminal 字體

```json
{
    ......
"terminal.integrated.fontFamily": "MesloLGS NF"
    ......
}
```

設定 GNOME terminal 字體

![alt tag](https://i.imgur.com/OTKTiXC.png)

設定 Konsole 字體

![alt tag](https://i.imgur.com/lvD5QVE.png)

## 安裝 Powerlevel10k

可參考 [https://github.com/romkatv/powerlevel10k#oh-my-zsh](https://github.com/romkatv/powerlevel10k#oh-my-zsh)

```cmd
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

接著執行 `vim ~/.zshrc`, 修改主題 `ZSH_THEME="powerlevel10k/powerlevel10k"`.

重新打開 terminal 就會跳出設定介面了.

![alt tag](https://i.imgur.com/8KiD5od.png)

如果沒有自動跳出設定的介面, 可以手動執行 `p10k configure`.

## p10k 設定 Enable Transient Prompt

說明一下這個設定

![alt tag](https://i.imgur.com/3irp0A6.png)

如果你選 no, 就和一般的一樣

![alt tag](https://i.imgur.com/A547n6g.png)

如果你選 yes, 你輸入的指令永遠都會在最下方

![alt tag](https://i.imgur.com/JrNqbKs.png)