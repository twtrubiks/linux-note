# Ubuntu 利用 imwheel 修正滾輪速度

Ubuntu 系統預設無法調整滑鼠滾輪速度，

所以有時候會覺得滑鼠在 Ubuntu 上很難用。

這時候可以透過第三方套件 `imwheel` 來解決。

## 安裝 imwheel

```shell
sudo apt install imwheel
```

## 設定

編輯 `~/.imwheelrc`，

```shell
vim ~/.imwheelrc
```

`~/.imwheelrc` 內容為，

```shell
".*"
None, Up, Button4, 5
None, Down, Button5, 5
Shift_L,   Up,   Shift_L|Button4, 4
Shift_L,   Down, Shift_L|Button5, 4
Control_L, Up,   Control_L|Button4
Control_L, Down, Control_L|Button5
None,  Thumb1, Control_L|C
None,  Thumb2, Control_L|V
```

在第 2，3 行中的 5 代表是滾輪速度，

數字越大，速度越快，請自行調整。

最下面兩行是開啟滑鼠側鍵 ( 如果有 ) 功能。

## 執行

```shell
imwheel
```

修改設定後請重新執行

```shell
killall imwheel
imwheel
```

![alt tag](https://i.imgur.com/g1bQs3v.png)

接著就會發現滑鼠好用多了。

## 設定開機自動啟動

在 Startup Application 中加入 imwheel 指令。

![alt tag](https://i.imgur.com/iS2MuQ0.png)

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

綠界科技ECPAY ( 不需註冊會員 )

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[贊助者付款](http://bit.ly/2F7Jrha)

歐付寶 ( 需註冊會員 )

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

