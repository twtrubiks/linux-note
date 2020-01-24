# Ubuntu Linux 修正 `E: Could not get lock /var/lib/dpkg/lock` Error

在使用 Ubuntu Linux 時, 看到類似以下錯誤訊息該怎麼修正

```cmd
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```

![alt tag](https://i.imgur.com/gUwGRMh.png)

## 原因

會發生這個錯誤是因為當我們嘗試更新, 安裝軟體時, 系統會去 lock dpkg ( Debian package manager ) file,

當 locking 時, 多個 processes 是不能同時去修改一個內容的.

所以, 當有時候執行更新, 安裝軟體時, 不小心不正確中斷 (可能是不正常關機, 或是不小心把 terminal 關閉),

又或是開機時它剛好在檢查更新, 然後你又使用 terminal 去執行更新的動作,

也都很容易跳出這個錯誤:unamused:

## 解決方法

請執行以下指令

```cmd
sudo lsof /var/lib/dpkg/lock
sudo lsof /var/lib/apt/lists/lock
sudo lsof /var/cache/apt/archives/lock
```

執行上述指令時, terminal 可能不會回傳任何的東西, 也可能會回傳一個數字 ( process_id ),

請使用以下指令把 process_id kill

```cmd
sudo kill -9 <process_id>
```

接下來移除被 lock 住的 files

```cmd
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```

移除完之後, 重新設定 packages

```cmd
sudo dpkg --configure -a
```

接著再去執行你原先想要執行的指令 ( 就可以正常 work 了 ):smile:

