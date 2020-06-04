# openvpn tutorial

[Youtube Tutorial - openvpn tutorial]()

## 教學

為了方便說明, 先假設 A 為 VPN server, B 為 client 端.

### vpn server 端

建議可以先執行更新

```cmd
sudo apt-get update
```

請參考 [openvpn-install github](https://github.com/angristan/openvpn-install)

```cmd
curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
chmod +x openvpn-install.sh
```

```cmd
AUTO_INSTALL=y ./openvpn-install.sh
```

如果順利, 路徑下應該會多出 `client.ovpn` 這個檔案,

請將這個檔案傳到你的 client 端, client 端將透過這個檔案連接 vpn.

建議執行

```cmd
systemctl enable openvpn
```

### client 端

安裝 openvpn

```cmd
sudo apt install openvpn
```

使用 openvpn 連接 server 端的 vpn

( 記得使用 `sudo` 連接 )


```cmd
sudo openvpn client.ovpn
```

確認是否成功連線 vpn , 可使用以下指令確認

```cmd
curl ifconfig.me
```