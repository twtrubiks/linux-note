# linux-nfs-server - 如何在 ubuntu 啟用 NFS Server

NFS (Network File System)

NFS 的優點是在 linux 上佈署簡單, 傳輸速度(相對 FTP)快.

NFS 的缺點就是加密的部份, 因為單純只是使用 IP 而已, 相對 FTP 來說比較不安全.

(但如果搭配內網以及防火牆應該會安全很多:relaxed:)

## server 端

在這邊我是使用 ubuntu 18.04,

安裝 NFS Server

```cmd
sudo apt install nfs-kernel-server
```

建立要分享的資料夾 ( NFS mount directory )

```cmd
sudo mkdir -p /mnt/nfs_shared
```

因為目標是讓 client 端可以訪問 shared directory, 所以移除資料夾權限以及 gropus

```cmd
sudo chown -R nobody:nogroup /mnt/nfs_shared/
```

接著再給它最大 777 權限,

如果不懂, 可參考之前的 [chmod 介紹](https://github.com/twtrubiks/linux-note#chmod)

```cmd
sudo chmod 777 /mnt/nfs_shared/
```

nfs 權限以及 ip 定義在 `/etc/exports` 中

```cmd
sudo vim /etc/exports
```

![alt tag](https://i.imgur.com/RosnyH1.png)

如果要設定很多 ip 就多寫幾行

```cmd
/mnt/nfs_shared  client_IP_1 (re,sync,no_subtree_check)
/mnt/nfs_shared  client_IP_2 (re,sync,no_subtree_check)
```

`rw` 設定讀寫權限.

`ro` 設定唯讀.

`sync` 資料同步寫入到記憶體與硬碟當中.

`async` 資料會先暫存於記憶體當中，而非直接寫入硬碟.

Export the NFS Share Directory

```cmd
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

設定 Firewall

這部份建議要做, 安全一點

```cmd
sudo ufw allow from client_IP to any port nfs
```

確認防火牆狀態

```cmd
sudo ufw enable
sudo ufw status
```

## client 端

安裝 NFS Client

```cmd
sudo apt install nfs-common
```

建立資料夾

```cmd
sudo mkdir -p /mnt/nfs_client
```

將 server 端的 nfs_shared 資料夾掛載

```cmd
sudo mount server_IP:/mnt/nfs_shared  /mnt/nfs_client
```

如果可以在資料夾裡面讀寫資料就是成功了哦:smile:

## client 端如何刪除掛載資料夾

要先卸除掛載, 才能刪除

```cmd
sudo umount nfs_client
sudo rm -rf nfs_client
```

## 查 ip 指令

```cmd
ip addr
```

```cmd
ifconfig
```
