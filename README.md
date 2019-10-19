# linux-note

主要是紀錄一些 linux 的指令📝

本篇文章會一直更新，用來告訴自己 Linux 有多爛 :scream:

* [籌備中](xxx)

## cd

切換到家目錄 `~`

```cmd
cd ~
```

切換到根目錄 `/`

```cmd
cd /
```

## ls

列出檔案

```cmd
ls -l
```

`-l` 顯示詳細的資訊 ( 檔案權限 )。

在 Linux 中，檔案都擁有四種權限

* 可讀取 ( r，Readable )，用數字 4 表示。

* 可寫入 ( w，writable )，用數字 2 表示。

* 可執行 ( x，eXecute )，用數字 1 表示。

* 無權限 ( - )，用數字 0 表示。

為了更清楚，我把它整理成表格:yum:

|     字元     | 權限分數 |
|:------------:|:--------:|
|   r (read)   |     4    |
|   w (write)  |     2    |
|  x (execute) |     1    |
|    - 無權限  |     0    |

如下圖所示，

![alt tag](https://i.imgur.com/AzfYBhf.png)

接著說明裡面每一欄的意思，

![alt tag](https://i.imgur.com/3TMcAtC.png)

* 第一欄 ( 圖上編號 1 )，使用者權限。

由 10 個字元組成，

第一個字元代表檔案型態 (`-`為檔案，`d`為目錄，`1` 為連結檔案 )。

第二、三、四個字元 表示檔案擁有者的存取權限。

第五、六、七個字元 表示檔案擁有者所屬群組成員的存取權限。

第八、九、十個字元 表示其他使用者的存取權限。

來看一個例子，drwxr-xr-x，

代表它是一個檔案，

擁有者具備讀、寫、執行權限，

所屬群組只擁有讀、執行權限，

其他使用者只擁有讀、執行權限。

為了更清楚，我把它整理成表格:yum:

|                |        擁有者        |      所屬群組      |     其他使用者     |
|----------------|:--------------------:|:------------------:|:------------------:|
|        d       |          rwx         |         r-x        |         r-x        |
| 代表是一個檔案 | 具備讀、寫、執行權限 | 只擁有讀、執行權限 | 只擁有讀、執行權限 |

它的權限分數是 755

|  身份  	| 權限 	|   分數   	|
|:------:	|:----:	|:--------:	|
|  owner 	|  rwx 	| 4+2+1 =7 	|
|  group 	|  r-x 	| 4+0+1 =5 	|
| others 	|  r-x 	| 4+0+1 =5 	|

* 第二欄 ( 圖上編號 2 )，檔案數量。

* 第三欄 ( 圖上編號 3 )，擁有者。

* 第四欄 ( 圖上編號 4 )，群組。

* 第五欄 ( 圖上編號 5 )，檔案大小。

* 第六欄 ( 圖上編號 6 )，檔案建立時間。

* 第七欄 ( 圖上編號 7 )，檔案名稱。

ls 使用時間排序

```cmd
ls -t
```

列出特定檔案 ( 列出為 .py 的檔案 )

```cmd
ls *.py
```

`-h` 參數，使用 KB、MB、GB 單位顯示檔案或目錄大小。

```cmd
ls -l -h
```

## chmod

改變檔案權限

```cmd
chmod XXX filename
```

舉個例子，將權限設為 rw-rw-r--，

|  身份  	| 權限 	|   分數   	|
|:------:	|:----:	|:--------:	|
|  owner 	|  rw- 	| 4+2+0 =6 	|
|  group 	|  rw- 	| 4+2+0 =6 	|
| others 	|  r-- 	| 4+0+1 =4 	|

```cmd
chmod 664 README.md
```

常用修改權限的指令

```cmd
# 只有擁有者 owner 有讀和寫的權限
sudo chmod 600 ×××
```

```cmd
# 擁有者 owner 有讀和寫的權限，group，others 只有讀的權限
sudo chmod 644 ×××
```

```cmd
# 擁有者 owner 有讀和寫以及執行的權限
sudo chmod 700 ×××
```

```cmd
# 擁有者 owner，group，others 都有讀和寫的權限
sudo chmod 666 ×××
```

```cmd
# 擁有者 owner，group，others 都有讀和寫以及執行的權限，基本上就是全開
sudo chmod -R 777 xxx
```

`-r` `-R` 代表 recursive 遞迴 ( 目錄底下所以檔案包含子目錄都會變更 )，

還有一種方法是使用 符號 來改變權限，

```cmd
chmod ug+x README.md
```

更詳細的部分我以後再來說:wink:

## chown

修改檔案或目錄的擁有者與群組。

修改檔案或目錄的擁有者

```cmd
# 將 README.md ( 檔案 ) 的擁有者改為 twtrubiks ( 使用者 )
chown twtrubiks README.md
```

修改檔案或目錄的群組

```cmd
# 將 README.md ( 檔案 ) 的群組改為 twtrubiksgroup ( 群組 )
chown :twtrubiksgroup README.md
```

同時修改檔案或目錄的擁有者和群組

```cmd
# 將 README.md ( 檔案 ) 的擁有者改為 twtrubiks ( 使用者 ) 以及
# 將 README.md ( 檔案 ) 的群組改為 twtrubiksgroup ( 群組 )
chown twtrubiks:twtrubiksgroup README.md
```

## zip unzip

```cmd
sudo apt-get install zip unzip
```

zip

```cmd
zip -r <壓縮後的檔名> <壓縮的檔案>
zip -r file.zip file
```

unzip

```cmd
unzip <解壓縮的檔案> -d <解壓縮的目標資料夾>
unzip file.zip -d zip_extract
```

如果希望直接解壓縮到當前的目錄，可以直接使用 `.`

```cmd
unzip file.zip -d .
```

## wget

下載工具

```cmd
sudo apt-get install wget
```

下載 URL 指令

```cmd
wget http://ftp.gnu.org/gnu/wget/wget-1.20.3.tar.gz
```

指定檔名，請加上 `-O`

```cmd
wget -O wget.tar.gz http://ftp.gnu.org/gnu/wget/wget-1.20.3.tar.gz
```

## scp

這個方法適用於 Linux 和 Linux 之間互傳檔案，也適用於  Linux 和 Windows 之間互傳檔案，

假設，Linux ip 為 192.168.56.101，查看 ip 指令如下，

```cmd
ip addr show
```

![alt tag](https://i.imgur.com/AlAeRoD.png)

確認有安裝 openssh-server

```cmd
sudo apt-get install openssh-server
```

使用 `ssh localhsot` 測試

![alt tag](https://i.imgur.com/nYo5NNn.png)

一切正常之後。

從 Windows 上傳送檔案給 Linux ( ip 為 192.168.56.101 )，

在 Windows 上的 cmd 執行以下指令，

```cmd
scp -rp 檔案 linux的使用者@ip:目標路徑
```

```cmd
scp -rp file twtrubiks@192.168.56.101:/home/twtrubiks
```

![alt tag](https://i.imgur.com/0nBrt00.png)

接下來，從 Linux 上拿檔案回 Windows

```cmd
scp -P 22 linux的使用者@ip:目標路徑 存放的目標位置
```

```cmd
scp -P 22 twtrubiks@192.168.56.101:/home/twtrubiks/linux_file.md .
```

`.` 代表當下目前路徑 ( 也可以指定其他的路徑 )。

![alt tag](https://i.imgur.com/aMnNlGI.png)

Linux 之間的傳送也是相同的道理:smile:


## mv

move ( rename ) files，**移動檔案**或是**重新命名檔案**。

修改 資料夾 or 檔案 檔名

```cmd
mv folder folder-new
mv README.md README_MV.md
```

移動檔案

```cmd
mv README.md /examples
```

移動檔案

```cmd
mv file.md example/
```

## rm

刪除檔案

```cmd
rm file.md
```

刪除資料夾

```cmd
rm -rf mydir
```

`-r` 代表使用 recursive 遞迴刪除。 ( 會將目錄內所有檔案刪除 )

`-f` 代表強制刪除 ( 不會跳出警告 )。

刪除特定的副檔名

```cmd
rm -f *.zip
```

也可以這樣

```cmd
rm -f *demo.zip
```

## cp

複製資料夾

```cmd
cp -r path_to_source/ path_to_destination/
```

`-r` `-R` 代表 recursive 遞迴，

如果 path_to_destination 不存在，會自動建立 ;

如果存在，則直接使用。

只想複製資料夾底下的全部內容，

```cmd
cp -r dir_1/. dir_2
cp -r dir_1/. .
```

`.` 代表資料夾內的東西，也可以代表目前所在的地方。

有時候會希望複製時可以保存當時的權限，所以會加上 `-p`。

```cmd
cp -r --preserve=all path_to_source/ path_to_destination/
```

`-p` `--preserve` 代表一同複製當下的權限以及擁有者之類的。

## find

查詢檔案

找檔案或資料夾

```cmd
sudo find / -name "dir-name"
sudo find / -name "file-name"
sudo find / -name "*.conf"
```

在當前目錄下尋找檔名為 README.md

```cmd
find . -name README.md
```



## tail

顯示檔案最後幾行內容

```cmd
tail README.md
```

持續顯示更新內容，通常使用在 server 或看 log

```cmd
tail -f README.md
```

## file

檢查檔案類型

```cmd
file README.md
```

## cat

將檔案內容顯示在 terminal 上

```cmd
cat README.md
```

## mkdir

建立資料夾

```cmd
mkdir -p dir1/dir2
```

`-p` `--parents`  代表自動建立上層目錄，如果目錄已存在則不會發生錯誤。

## 不用密碼遠端登入 Linux

先確認 Linux 上有 `.ssh` 資料夾，如果沒有，

請使用以下指令建立 ( 以及權限 )，

```cmd
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

每次遠端登入 Linux 都需要密碼很麻煩，有沒有可以透過其他的方式不要輸入密碼:question:

有，先在本機電腦使用 `ssh-keygen` 產生金鑰

```cmd
ssh-keygen
```

接著你會有兩把 key ,

id_rsa.pub：公開金鑰（public key）: 要放在遠端的 Linux 伺服器上。

id_rsa：私密金鑰（private key）: 自己保護好，等同於你的 Linux 密碼。

先把自己本機 `id_rsa.pub` 傳到遠端的 Linux server 上，

接著在 Linux 上執行以下指令

( 把公開金鑰放到 `~/.ssh/authorized_keys` )

```cmd
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

讓我們來測試看看吧:smile:

`ssh twtrubiks@192.168.56.101`

![alt tag](https://i.imgur.com/97ndrP8.png)

不用輸入密碼就可以登入了:thumbsup:

## root 使用者登入遠端 Linux

雖然這個方法可以比較危險，但我還是說明一下:joy:

先設定 root 密碼，執行以下指令

```cmd
sudo passwd root
```
![alt tag](https://i.imgur.com/dsSrBJX.png)

再使用 `su -` 測試 root 密碼

![alt tag](https://i.imgur.com/nmU9cgk.png)

測試完成後，再執行 `ssh root@192.168.56.101`

![alt tag](https://i.imgur.com/BF4JWnO.png)

你會發現一直出現 Permission denied, please try again.

這時候要到 Linux 上修改 root 的 ssh 設定，

```cmd
sudo vim /etc/ssh/sshd_config
```

找到 PermitRootLogin without-password

![alt tag](https://i.imgur.com/NO85xui.png)

修改成 PermitRootLogin yes

![alt tag](https://i.imgur.com/xpyfpwW.png)

注意，Linux 一定要重新啟動，否則無法生效。

成功使用 root 登入了:satisfied:

![alt tag](https://i.imgur.com/Au4wt32.png)


## 其他資訊

查看 cpu

```cmd
cat /proc/cpuinfo
```

查看全部的 ram 大小

```cmd
grep MemTotal /proc/meminfo
```

查看可用的 ram

```cmd
grep MemFree /proc/meminfo
```

也可以使用

```cmd
free -m
```

## Reference

* [Linux Command 命令列指令與基本操作入門教學](https://blog.techbridge.cc/2017/12/23/linux-commnd-line-tutorial/)

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

綠界科技ECPAY ( 不需註冊會員 )

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[贊助者付款](http://bit.ly/2F7Jrha)

歐付寶 ( 需註冊會員 )

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## 贊助名單

[贊助名單](https://github.com/twtrubiks/Thank-you-for-donate)

## License

MIT license

