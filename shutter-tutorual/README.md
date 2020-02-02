# Ubuntu 18.04 install Shutter

[Youtube Tutorial - Ubuntu 18.04 install Shutter](https://youtu.be/sDengQSUbxs)

Shutter is an excellent image capture tool:smile:

## install Shutter

Execute the following command

```shell
sudo apt-get update

sudo apt-get install shutter
```

## Fix Disable Edit In Shutter

On Ubuntu 18.04, you will find that the edit option is unavailable,

use the following command to fix ,

download directly and double-click to install.

[libgoocanvas-common](https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas-common_1.0.0-1_all.deb)

[libgoocanvas3](https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas3_1.0.0-1_amd64.deb)

[libgoo-canvas-perl](https://launchpad.net/ubuntu/+archive/primary/+files/libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb)


After the installation is complete, please restart your system or run `sudo killall shutter`.

you will find enable Edit

![alt tag](https://i.imgur.com/TYR8NnX.png)

## Show applet indicator for Shutter

Use the following command to enable Shutter applet indicator

```shell
sudo apt install libappindicator-dev
```

```shell
sudo cpan -i Gtk2::AppIndicator
```

Restart Shutter, and you should see the applet indicator on the top panel

![alt tag](https://i.imgur.com/aZPScXK.png)

## Set keyboard shortcuts

use commmand

```cmd
shutter -s
```

![alt tag](https://i.imgur.com/IDKq9xQ.png)
