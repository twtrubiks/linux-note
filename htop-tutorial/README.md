# htop

[Youtube Tutorial - Linux htop tutorial](https://youtu.be/_h90Z3WRc6s)

htop repo:[htop](https://github.com/hishamhm/htop)

本篇文章將會教大家使用超強的 `htop`,

你可能會說, 系統原先就有 `top` 了, 為什麼還要在另外裝:question:

我用圖片來說明這個原因:smile:

## 說明

這是內建的 `top`

![alt tag](https://i.imgur.com/d5r8aXm.png)

這是 `htop` ( 而且 htop 支援滑鼠哦)

![alt tag](https://i.imgur.com/knIl247.png)

我想答案明顯了:flushed: 肯定選 `htop`

先安裝 `htop`

```cmd
sudo apt-get install htop
```

使用 `htop` 的方式很簡單

```cmd
htop
```

![alt tag](https://i.imgur.com/DbE3T0y.png)

說明每個代號所代表的意思,

`PID` Process ID

`USER` 開啟 process 的 user name

`PRI` Priority，Linux kernal 優先順序，從 0(最高 Priority) 到 139(最低 Priority)

`NI` Niceness，數值從 -20(最高Priority) 到 19(最低Priority)

`VIRT` Virtual memory usage，虛擬記憶體

`RES` Resident memory usage，常駐記憶體

`SHR` Shared memory usage，共享記憶體

`S` Process state

- `R` 執行中或可執行 ( 或在 queue 中)

- `S` 可中斷的睡眠 ( 等待完成的事件 )

- `D` 不可中斷的睡眠狀態 ( IO )

- `Z` Zombie Process，殭屍程序

- `T` 工作停止

- `t` 除錯中斷

`CPU%` CPU 使用率

`MEM%` 記憶體(Ram) 使用率

`TIME+` process 執行的時間

`Comamnd` 執行的 comamnd

方向按鍵, 上下左右都可以按, PgUP, PgDn, Home, End 也都可以.

最下面那一排有 F1~F10 執行對應的動作

![alt tag](https://i.imgur.com/B8KZtj6.png)

`F2` 設定

![alt tag](https://i.imgur.com/fTV6Gus.png)

`F3` or `/` 搜尋內容, 直接輸入你要找的東西即可

![alt tag](https://i.imgur.com/iYue5h8.png)

`F4` Filter, 會慢慢濾掉你輸入的內容

![alt tag](https://i.imgur.com/MNg2wkW.png)

不管是 search 還是 filter, 都可以用 Esc 來取消功能

`F5` Tree 模式

![alt tag](https://i.imgur.com/ecpnMHk.png)

`[` or `F7`  Nice – (change priority), 減少 nice 值, 提高 process 的優先順序

`]` or `F8`  Nice + (change priority), 增加 nice 值, 降低 process 的優先順序

如果不了解為什麼減少 nice 值, 提高 process 的優先順序的原因, 只要想成是對其他的

process 不 nice (所以優先權自然就提高了)。

![alt tag](https://i.imgur.com/TtzLoZ7.png)

如果發現無法調整 Nice, 請使用 `sudo htop` 執行。

`h` help (或按F1)

![alt tag](https://i.imgur.com/Wx58Y6i.png)

`Space` 標記/取消標記 process, 可搭配其他指令, 例如將標記的全部 `Kill`

![alt tag](https://i.imgur.com/34RffTF.png)

`U` 取消全部標記 process

`u` 選擇顯示特定用戶 process

![alt tag](https://i.imgur.com/f2PBpWE.png)

`M` 依照 Memory 排序

`P` 依照 CPU 使用排序

`T` 依照 TIME+ 排序

`F` 追蹤模式，可以對某個 process 進行追蹤, 不管排序如何改變, 它都會顯示在螢幕可見的地方, 非常方便。

如下圖黃色的地方, 代表進入追蹤模式

![alt tag](https://i.imgur.com/UGX3i1o.png)

`直接輸入數字鍵` 代表快速定位到 Numbers PID 上 (不會顯示輸入狀態)

![alt tag](https://i.imgur.com/RHkNahw.png)

`Ctrl+L` 重新整理.

也可以透過 terminal 直接執行需要的指令

`-u` 顯示特定用戶 process, 顯示 `twtrubiks` user 的 processes

```cmd
htop -u twtrubiks
```

`-s` 以指定的方式排序, 以 PPID 的方式進行排序

```cmd
htop -s PPID
```

## 其他

避免重複顯示，

將 Display options 中的 Hide userland process threads 選起來,

這樣就會不有重複顯示的問題.

![alt tag](https://i.imgur.com/6UbzFpZ.png)

## 如何回到預設值

不管我們做了那些修改, 都會保存在以下的地方,

```cmd
$HOME/.config/htop/htoprc
/home/twtrubiks/.config/htop/htoprc
```

如要回到預設值，直接刪除 htoprc 即可。
