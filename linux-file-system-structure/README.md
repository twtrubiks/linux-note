# Linux 檔案系統(結構)

[Youtube Tutorial - Linux 教學 - 檔案系統(結構)](https://youtu.be/C-1ebRFBiAI)

這篇紀錄 Linux 檔案系統(結構), 只會說一些比較重要的:smile:

Linux目錄結構

## `/` 根目錄

`/bin`

bin 是 binary 的縮寫, 裡面通常是可執行的檔案, 像是 `cp` `ls` `ln` 這些指令都是保存在該目錄底下.

`/boot`

這裡是啟動 Linux 時需要的核心檔案, efi, grub 都在這裡面.

`/dev`

device 的縮寫, 系統設備裝置檔案都在此目錄中, 像是 usb, cpu, disk, sda, ide, ram.

`/etc`

此目錄存放系統以及軟體的設定檔 `.conf`, `sources.list.d` 這類儲存 repo 的資訊也都在這裡面,

修改目錄底下的內容時請記得加上 `sudo`.

`/etc/passwd`

紀錄系統中每一個 user 的行為, 包含 User ID, Group ID.

`/home`

每個 user 的個人文件, 每個 user 的主目錄都是以自己的名稱命名, 像是 `/home/twtrubiks` (username 就是 twtrubiks)

`/lib`

類似 windows 內的 `.dll` 檔案, 核心函式庫 (library).

`/lib64`

64 位元的系統會有這個資料夾, 核心函式庫(library).

`/lost+found`

系統如果不正常產生錯誤時, 會將那些丟失的片段放在這個目錄中, 通常這個目錄會自動出現在裝置的目錄下,

就像是你的 usb 中都會看到這個目錄, 它是砍不掉的目錄.

`/media`

可移動的掛載點, 通常會自動把額外的硬碟 or usb 掛載到該文件下.

`/mnt`

使用在臨時掛載的地方, 通常這個目錄底下是空的.

`/opt`

多數的第三方軟體會安裝到這邊, 像是 brave, teamviewer, 如果你想測試最新的軟體, 可以直接安裝在

該目錄底下, 測試完之後, 直接軟體目錄刪除即可, 不會影響到系統的運作, 並不是每個系統都會建立這個目錄.

`/proc`

此目錄底下為系統核心和程式執行的狀態, 大多為文檔, 像是 `/proc/cpuinfo` 保存了有關 CPU 的資訊.

`/root`

根用戶的主目錄, 和 `/home` 類似.

`/sbin`

system binary 的縮寫, 通常是系統管理員才能訪問, 因為這裡面多數都是系統管理指令,

像是 `shutdown` `reboot` `ip` `mount` `fdisk`.

`/tmp`

該目錄是讓一般使用者暫時存放檔案的地方, 有些軟體就是使用 `/tmp` 當作預設的目錄, 記住, 重要資料

不要放此目錄, 通常在這資料夾的東西檔案是比較小且存放的時間比較短的, 不少系統清除 `/tmp`

是非常快的, 甚至實現把 `/tmp` 存放在 ram 中 (加快讀寫速度).

## `/usr`

Unix Software Resource 的縮寫,

`/usr` 是系統核心所在, 很重要的一個目錄，包含了相當多的系統資訊, 程式, 指令.

以下介紹 `/usr` 目錄結構,

`/usr/bin`

所有可執行的檔案, `sudo` `ssh` `ssh-agent` `gcc` `python` 這些都在裡面.

`/usr/sbin`

類似 /sbin.

`/usr/share`

共享文件都在此目錄, 像是字體, 檔案, icon.

`/usr/share/icons`

應用程序的 icon.

`/usr/share/fonts`

字體文件, 當前用戶的字體可以是在 `~/.fonts` 底下.

`/usr/local`

當你安裝完 Linux 之後, 安裝軟體(手動安裝), 預設的安裝位置就是 `/usr/local`, 如果是更新,

通常位置會在 `/usr/local/bin` 這裡 (與原先的做區別).

`usr/src`

linux 內核的程式源碼和說明文件.

## `/var`

變量(動態)文件, 包含系統運行時要改變的數據資料 (像是系統日誌).

`/var` 的存在讓 `/usr` 目錄只為讀的掛載可能.

以下介紹 `/var` 目錄結構,

`/var/cache`

應用程式的快取.

`/var/lib`

該目錄保存系統或某個 process 運行的狀態, 用戶不允許去修改裡面的內容.

`/var/local`

存放 `/usr/local` 中應用程式的動態(可變)資料.

`/var/lock`

鎖文件, 當某個 process 發現這個鎖的時候, 將不會嘗試去使用這個文件.

`/var/log`

系統日誌文件.

`/var/run`

保存 process 的 PID 編號.

`/var/crash`

當一個應用程式掛掉的時候, 可以透過此文件查看原因.

`/var/tmp`

暫存的目錄, 通常在這資料夾的東西檔案是比較大且存放的時間比較長的, 有些系統會自動清除目錄底下的內容.

