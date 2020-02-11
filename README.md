# linux-note

主要是紀錄一些 linux 的指令📝

( 本篇文章會持續更新:smile: )

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

回到上層目錄

```cmd
cd ..
```

## man

線上說明手冊 ( man page )

```cmd
man ls
```

![alt tag](https://i.imgur.com/3DDi208.png)

也可以使用

```cmd
ls --help
```

![alt tag](https://i.imgur.com/ZSZjuVC.png)

## pwd

查看目前的路徑

```cmd
pwd
```

## ls

列出檔案

```cmd
ls -l
```

`-l` 顯示詳細的資訊 ( 檔案權限 )。

也等於直接輸入 (L 的小寫)

```cmd
ll
```

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

顯示全部的檔案 (包含隱藏檔)

```cmd
ls -a
```

也可以使用

```cmd
ls -al
```

可以直接列出資料夾內的內容

```cmd
ls Downloads
```

像是在 home 底下, 直接列出 Downloads 內容

![alt tag](https://i.imgur.com/Dal7aSn.png)

sort

```cmd
ls -S
```

將輸出結果寫到檔案裡

```cmd
ls -lS > file.txt
```

## su

切換不同的 user

```cmd
su <username>
```

## sudo

增加新的 user

```cmd
sudo useradd <username>
```

設定 user 的 password

```cmd
sudo passwd <username>
```

刪除 user

```cmd
sudo userdel <username>
```

增加新的 group

```cmd
sudo groupadd <groupname>
```

刪除 group

```cmd
sudo groupdel <groupname>
```

增加 user 到 group 中

```cmd
sudo usermod -g <groupname> <username>
```

查看所有 user

```cmd
cat /etc/passwd
```

查看所有 group

```cmd
cat /etc/group
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

在介紹之前，先看下方的表格 :wink:

|       | u = user  |          |             |              |
|-------|-----------|----------|-------------|--------------|
|       | g = group | + (增加) | r = read    |              |
| chmod |           | - (移除) | w = write   | 檔案或資料夾 |
|       | o = other | = (設定) | x = execute |              |
|       | a = all   |          |             |              |

舉個例子，將 hello 權限設為 rw-rw-r--，

|  擁有者(u)  	| 所屬群組(g) 	|   其他使用者(o)   	|
|:------:	|:----:	|:--------:	|
|  rw- 	|  rw- 	| r-- 	|

```cmd
chmod ug=rw,o=r hello
```

![alt tag](https://i.imgur.com/QgNuNel.png)

再舉個例子，將 hello 權限設為 rwxr-xr–-，

```cmd
chmod u=rwx,g=rx,o=r hello
```

![alt tag](https://i.imgur.com/WlX8wPL.png)

接著假設我希望把 可執行的權限(x) 加上去 (全部人及群組都加上)

```cmd
chmod a+x hello
```

![alt tag](https://i.imgur.com/KLiwPXX.png)

移除所有人 可執行的權限(x)

```cmd
chmod a-x hello
```

你會發現大家的 可執行的權限(x) 都消失了

![alt tag](https://i.imgur.com/O8gh3Is.png)

相信經過這一連串的練習，大家肯定了解了，

如果不懂，多看幾遍:satisfied:

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

## ln

[Youtube Tutorial - Linux 指令教學-ln (Symbolic Link)](https://youtu.be/jdZsO2GAf2I)

有兩種, 分別為 hard link 和 Symbolic link ( soft link ),

先介紹 hard link，注意，hard link not allowed for directory。

```cmd
ln /home/twtrubiks/Downloads/odoo-git/README.md
```

![alt tag](https://i.imgur.com/ioJXBRw.png)

hard link 特性為不管刪除哪一個檔案，檔案都會被保留。除非你把最後一個檔案也刪除，

換個方式說，一個檔案的 hard link 和本來的檔案其實沒有任何實質上的區別。

hard link 不允許資料夾，只允許檔案。

symbolic link，也稱 soft link，基本上它類似於 Windows 中的捷徑:smile:

```cmd
ln -s /home/twtrubiks/Downloads/odoo-git/ dir-link
```

![alt tag](https://i.imgur.com/JGhlQZd.png)

當某個檔案的的本體被刪除後，它的 symbolic link 就無法讀取到這個檔案了，

一個檔案的 symbolic link 和檔案的本體是不同的兩個東西。

symbolic link 允許檔案和資料夾。

## zip unzip

zip **不會**保存檔案的 permissions and ownership.

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

## tar

tar **會**保存檔案的 permissions and ownership.

壓縮 `.tar` format

```cmd
tar cvf filename.tar source-folder
```

解壓縮 `.tar` format

```cmd
tar xvf filename.tar
```

## unrar

```cmd
sudo apt-get install unrar
```

將 filename.rar 解壓縮到目錄底下

```cmd
unrar e filename.rar
```

列出 filename.rar 的資料

```cmd
unrar l filename.rar
```

測試 filename.rar 是否完整且正確

```cmd
unrar t filename.rar
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

全名為 Securely Copy,

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

`-r` 代表 recursive.

`-p` 代表 保存原始檔案的內容 (Preserves modification).

```cmd
scp -rp file twtrubiks@192.168.56.101:/home/twtrubiks
```

![alt tag](https://i.imgur.com/0nBrt00.png)

接下來，從 Linux 上拿檔案回 Windows

```cmd
scp -P 22 linux的使用者@ip:目標路徑 存放的目標位置
```

`-P` 代表明確指定連接的 port (remote host).

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

```cmd
mv file.md example/
```

其他的參數說明(參數可以多個一起使用)，

互動模式 , CLI 會詢問你是否 overwriting files

```cmd
mv -i source_file path_to_destination/
```

只更新來源資料夾和目的地不同的檔案

```cmd
mv -u source_file path_to_destination/
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

或是使用 rmdir 指令，

```cmd
rmdir mydir_name
```

不過要注意，被移除的資料夾裡面必須是空的，否則回無法移除。

刪除特定的副檔名，

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

其他的參數說明(參數可以多個一起使用)，

互動模式 , CLI 會詢問你是否 overwriting files

```cmd
cp -i source_file path_to_destination/
```

不詢問 , 直接 overwriting files

```cmd
cp -n source_file path_to_destination/
```

只更新來源資料夾和目的地不同的檔案

```cmd
cp -u source_file path_to_destination/
```

印出資訊

```cmd
cp -v source_file path_to_destination/
```

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

## source

source 指令通常用於剛修改的初始化文件, 讓它立刻生效, 不必重開機(或登出再登入),

以下例子,

```cmd
source demo.sh
```

在當下的 shell 內去讀取, 執行 demo.sh, 而 demo.sh **需要**有執行權限

(執行權限代表 `chmod +x demo.sh`)

source 指令也可以簡寫為 `.`

```cmd
. demo.sh
```

## sh or bash

使用 `sh` or `bash`執行時, **不需要**有執行權限.

(執行權限代表 `chmod +x demo.sh`)

```cmd
sh demo.sh
bash demo.sh
```

## ./

直接使用 `./` 執行, **需要**有執行權限.

(執行權限代表 `chmod +x demo.sh`)

當你執行

```cmd
./demo.sh

chmod +x demo.sh
./demo.sh
```

你會發現跳出類似訊息 `bash: ./demo.sh: Permission denied`,

修正方法如下,

```cmd
chmod +x demo.sh
./demo.sh
```

## where

尋找路徑，

舉例，尋找 python3 路徑

```cmd
where python3
which python3
whereis python3
```

## tail

顯示檔案最後幾行內容

```cmd
tail README.md
```

一次顯示多個檔案

```cmd
tail README_1.md README_2.md
```

指定顯示檔案最後 N 行內容

```cmd
tail -n 5 README.md
```

```cmd
tail README.md -n 5
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

顯示行數

```cmd
cat -n README.md
```

cat 也可以寫入檔案

```cmd
cat <<EOT >> hello_4.txt
line 1
line 2
line 3
EOT
```

## clear

clear the terminal screen ， 快捷鍵為 Ctrl+L

```cmd
clear
```

## grep

```cmd
# 格式
grep match_pattern file_name
```

```cmd
# 格式
grep "search name" README.md
```

也可以一次搜尋多個檔案

```cmd
grep "name" README_1.md README_2.md
```

也可以使用 萬用字元 `*`

```cmd
grep "print" *.py
```

搜尋當下目錄資料夾內容

```cmd
grep -r "search name" .
```

case insensitive case (不區分大小寫)

```cmd
grep -i "name" README_1.md
```

顯示行數

```cmd
grep -n "name" README_1.md
```

## mkdir

建立資料夾

```cmd
mkdir -p dir1/dir2
```

`-p` `--parents`  代表自動建立上層目錄，如果目錄已存在則不會發生錯誤。

## history

歷史輸入的指令

```cmd
history
```

```cmd
history | less
```

![alt tag](https://i.imgur.com/0YKqS3Y.png)

假設今天我不想打指令, 可以直接輸入 `!`+ 數字, 會自動執行該指令.

```cmd
!1848
```

## echo

在 shell 中印出 shell 的值，

設定 EDITOR

```cmd
export EDITOR=vim
```

查看目前的 EDITOR，

```cmd
echo $EDITOR
```

查看目前的 shell，

```cmd
echo $SHELL
```

echo 也可以寫入檔案，

方法一

```cmd
echo "line 1" >> hello_1.txt
```

方法二 ( 寫入多行 )

```cmd
echo "line 1
line 2" >> hello_2.txt
```

方法三 ( 寫入多行 )

```cmd
{
    echo 'line1;'
    echo 'line2;'
} >> hello_3.txt
```

## 不用密碼遠端登入 Linux

### 方法一

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

### 方法二

也可以透過 `ssh-copy-id` 來完成,

```cmd
ssh-copy-id -i ~/.ssh/id_rsa.pub twtrubiks@192.168.56.101
```

![alt tag](https://i.imgur.com/eR5TIJ3.png)

其實不管是方法一還是方法二, 都只是把 key 加入 `/home/<user>/.ssh`

裡的 `authorized_keys` 而已:smile:

![alt tag](https://i.imgur.com/j4BRI1J.png)

## root 使用者登入遠端 Linux

注意, 通常不會這樣做:exclamation:

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

最後記得一定要重新啟動 sshd 讓它生效 (或是重開機)

```cmd
systemctl restart sshd
```

成功使用 root 登入了:satisfied:

![alt tag](https://i.imgur.com/Au4wt32.png)

## 正確保護 server

比較安全的作法通常是關閉 `PermitRootLogin` 以及 `PasswordAuthentication`,

然後只啟用 `PubkeyAuthentication` 的方式, 但這邊要注意, 一定要把你的 key 放到

server 上, 否則如果設定完不小心退出, 就很麻煩:expressionless:

( 因為不能用密碼登入, 又忘記將 key 放到 server 中 )

修改

```cmd
sudo vim /etc/ssh/sshd_config
```

禁止 root 登入, 將 `PermitRootLogin` 設為 `no`,

![alt tag](https://i.imgur.com/W6KBiXS.png)

禁止使用 password 登入, 將 `PasswordAuthentication` 設為 `no`,

![alt tag](https://i.imgur.com/L9WPRq5.png)

允許 `PubkeyAuthentication`, 設為 `yes`

![alt tag](https://i.imgur.com/iYyaAQ8.png)

補充, 還有一種是使用 PAM Authentication `UsePAM` ( AWS 就是使用這種方式 )

![alt tag](https://i.imgur.com/g3MdnKC.png)

如同說明, 如果希望只使用 PAM Authentication, 也可以把  `ChallengeResponseAuthentication` 設為 `no`.

最後記得重新啟動 sshd 讓它生效 (或是重開機)

```cmd
systemctl restart sshd
```

## 其他資訊

系統訊息

```cmd
uname -a
```

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

查看硬碟空間 ( disk free space, Human Readable )

```cmd
df -h
```

所有硬碟的訊息 ( List Drives and Mounts )

```cmd
lsblk
```

查看所有 pci，

```cmd
lspci
```

查看所有 usb，

```cmd
lsusb
```

也可以搭配 grep 搜尋，只搜尋包含 VirtualBox 的內容，

```cmd
lsusb | grep VirtualBox
```

查看 ip，

```cmd
ip a
```

external ip Address ( 對外的 ip )，`ifconfig.me` 是一個網站，

```cmd
curl ifconfig.me
```

查看電腦目前資訊，CPU RAM 等等

```cmd
top
```

推薦 `htop` ( 資訊更清楚 ), 建議參考 [htop-tutorial](https://github.com/twtrubiks/linux-note/tree/master/htop-tutorial) - htop tutorial:thumbsup:

透過 xrandr 修改螢幕的亮度，

先查看螢幕的 name，

```cmd
xrandr | grep " connected" | cut -f1 -d " "
```

設定亮度 ( 0 - 1 )，

```cmd
xrandr --output DP-1 --brightness 0.7
```

## install packages

install

```cmd
sudo apt-get install xxx
```

如果只有 `.deb` 檔案, 可以使用以下的方式

```cmd
sudo apt install ./xxx.deb
```

update list ( 更新 packages 的最新資訊及列表 )

```cmd
sudo apt-get update
```

upgrade ( 更新軟體到最新的版本 )

```cmd
sudo apt-get upgrade
```

remove

```cmd
sudo apt-get --purge autoremove xxxx
```

清理不需要的 packages ( `.deb` )

```cmd
sudo apt autoclean
```

清除不需要的依賴

```cmd
sudo apt autoremove
```

## remove snap

在 ubuntu 中, 預設會幫你安裝 snap, 但有些人不太喜歡,

因為他是私人公司為維護的:smile:

以下附上移除 snap 指令,

```cmd
sudo rm -rf /var/cache/snapd/
sudo apt autoremove --purge snapd gnome-software-plugin-snap
rm -rf ~/snap
```

## Linux 檔案系統(結構)

[Linux-File-System/Structure](https://github.com/twtrubiks/linux-note/tree/master/linux-file-system-structure)

## 更多文章

[zsh-tmux-tutorual](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual) - 超好用 zsh 以及 tmux。

[imwheel-tutorual](https://github.com/twtrubiks/linux-note/tree/master/imwheel-tutorual) - 改善 linux 滑鼠滾動問題。

[shutter-tutorual](https://github.com/twtrubiks/linux-note/tree/master/shutter-tutorual) - Shutter is an excellent image capture tool.

[systemctl-tutorial](https://github.com/twtrubiks/linux-note/tree/master/systemctl-tutorual)

[crontab-tutorual](https://github.com/twtrubiks/linux-note/tree/master/crontab-tutorual)

[mount-disks-at-system-startup-on-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/mount-disks-at-system-startup-on-ubuntu)

[chinese-input-methods-on-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/chinese-input-methods-on-ubuntu) - ubuntu 如何安裝中文輸入法

[gnome-tweaks](https://github.com/twtrubiks/linux-note/tree/master/gnome-tweaks) - Ubuntu 安裝 GNOME Tweak tool

[htop-tutorial](https://github.com/twtrubiks/linux-note/tree/master/htop-tutorial) - htop tutorial:thumbsup:

[neofetch-tutorial](https://github.com/twtrubiks/linux-note/tree/master/neofetch-tutorial) - command-line system information tool:thumbsup:

[how-to-change-login-lock-background-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/how-to-change-login-lock-background-ubuntu) - 修改Ubuntu 登入/鎖屏時的背景

## 狀況排除

[fix_could_not_get_lock_dpkg_ubuntu](https://github.com/twtrubiks/linux-note/tree/master/fix_could_not_get_lock_dpkg_ubuntu) - 修正 `E: Could not get lock /var/lib/dpkg/lock` Error

## 其他

[ubuntu-18-04-on-Lenovo-X1-Carbon-6](https://github.com/twtrubiks/linux-note/tree/master/ubuntu-18-04-on-Lenovo-X1-Carbon-6)

[透過 VirtualBox 安裝 Ubuntu 19.10 （以及一些個人想法）](https://youtu.be/lI1EMwhW6lE)

[VirtualBox 6.1 Linux 安裝 Guest Addition - 全屏教學](https://youtu.be/PMw6FPtbbaU)

[alternative-software](https://github.com/twtrubiks/linux-note/tree/master/alternative-software) - windows -> Linux 替代軟體

[rclone-tutorial](https://github.com/twtrubiks/linux-note/tree/master/rclone-tutorial) - rclone 是一套很棒的文件同步管理工具

[Linux 桌面環境 Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de)

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
