[中文版](README.md)

# Ubuntu 18.04 Install Shutter

[Youtube Tutorial - Ubuntu 18.04 install Shutter](https://youtu.be/sDengQSUbxs)

Shutter is an excellent image capture tool :smile:

## Install Shutter

Execute the following command:

**Ubuntu 18.04**

```shell
sudo apt-get update
sudo apt-get install shutter
```

**Ubuntu 20.04**

```shell
sudo apt-get update
sudo add-apt-repository -y ppa:linuxuprising/shutter
sudo apt-get install shutter
```

## Fix Disabled Edit Button in Shutter

(This can be ignored on Ubuntu 20.04, as the bug has been fixed :exclamation:)

On Ubuntu 18.04, you will find that the edit option is unavailable.
Use the following commands to fix it.

Download directly and double-click to install:

[libgoocanvas-common](https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas-common_1.0.0-1_all.deb)

[libgoocanvas3](https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas3_1.0.0-1_amd64.deb)

[libgoo-canvas-perl](https://launchpad.net/ubuntu/+archive/primary/+files/libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb)

After the installation is complete, please restart your system or run `sudo killall shutter`.

You will find that the Edit button is now enabled.

![alt tag](https://i.imgur.com/TYR8NnX.png)

## Show Applet Indicator for Shutter

Use the following command to enable the Shutter applet indicator:

```shell
sudo apt install libappindicator-dev
```

```shell
sudo cpan -i Gtk2::AppIndicator
```

Restart Shutter, and you should see the applet indicator on the top panel.

![alt tag](https://i.imgur.com/aZPScXK.png)

## Set Keyboard Shortcuts

Use the command:

```cmd
shutter -s
```

![alt tag](https://i.imgur.com/IDKq9xQ.png)