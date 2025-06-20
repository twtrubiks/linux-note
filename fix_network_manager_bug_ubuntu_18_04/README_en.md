[中文版](README.md)

# Fix for Missing Network Connection Settings in Ubuntu 18.04

Normally, you can configure the wired connection status in the red area shown in the image below.

![alt tag](https://i.imgur.com/3wLYDUA.png)

However, one time this red-boxed area disappeared on its own. After a long search, it seems to be a bug.

Here's a record of the fix. You just need to create an empty file named `10-globally-managed-devices.conf` in the path below to fix it.

```cmd
sudo touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
```

It is recommended to restart NetworkManager. A better method is to reboot (optional :relaxed:).

```cmd
sudo systemctl restart NetworkManager
```

For related discussions and explanations, you can refer to:

[https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1638842](https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1638842)
