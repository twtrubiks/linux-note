## 如何修改 Ubuntu 18.04 登入/鎖屏時的背景

[Youtube Tutorial-如何修改 Ubuntu 登入/鎖屏時的背景](https://youtu.be/k2Lzqz53-L0)

預設登入/鎖屏時的背景真的不太好看 :sweat_smile:

![alt tag](https://i.imgur.com/X7t9vCx.png)

但似乎沒地方可以修改, 教大家從哪裡修改 :smirk:

使用任何你能編輯文字的工具找到以下的路徑,

我個人是直接用 vscode (修改儲存時會和你要求權限)

```cmd
code /usr/share/gnome-shell/theme/ubuntu.css
```

或用其他的工具

```cmd
sudo vim /usr/share/gnome-shell/theme/ubuntu.css
```

找到以下的地方(把它註解)

```css
#lockDialogGroup {
  background: #2c001e url(resource:///org/gnome/shell/theme/noise-texture.png);
  background-repeat: repeat; }
```

然後換上自己的圖

```css
#lockDialogGroup {
    background: #2c001e url(file:///home/xxxxx);
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
}
```

如下

![alt tag](https://i.imgur.com/ncdc8hf.png)

需要重開機才會生效 :exclamation:

![alt tag](https://i.imgur.com/Qz2TEro.png)

但是有時候如果進行 ubuntu 的系統更新時, 會被洗掉(要重新設定), 有點小麻煩 :persevere: