[中文版](README.md)

# How to Enable Remote Desktop on Ubuntu

* [Youtube Tutorial - How to Enable Remote Desktop on Ubuntu](https://youtu.be/-01unOIk9mI)
* [Youtube Tutorial - Fix Ubuntu Remote Desktop Black Screen (No Monitor Connected)](https://youtu.be/3j4wUMX95zA)

## Installation Tutorial

This demonstration uses Ubuntu 18.04.

This uses the GNOME desktop (which is the default for most installations). If you don't know what a DE is, you can refer to [Linux Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de).

Please go to the following location to enable it:

![alt tag](https://i.imgur.com/QNjAP8e.png)

Then turn on Screen Sharing and set your 8-digit password.

Note, if you installed Ubuntu minimal, you will not see this option.
Just run the following command to install it and it will appear:

```cmd
sudo apt-get install vino
```

![alt tag](https://i.imgur.com/OjN7Prq.png)

At this point, your server side is actually set up.

Next, go to your client side (another computer).

First, install the client-side remote software. I personally use Remmina (Ubuntu will install this for you by default).

![alt tag](https://i.imgur.com/qGYgKGu.png)

The connection settings are as shown in the figure below. There are a few things to note:

Protocol: Select VNC

Server: Select your fixed IP. For testing, you can use `127.0.0.1`.

User name: This name is not important, you can enter anything.

User password: Enter the password you just set.

![alt tag](https://i.imgur.com/hekY4rz.png)

Connection successful:

![alt tag](https://i.imgur.com/ptfTAoh.png)

## Other Notes

* [Youtube Tutorial - Fix Ubuntu Remote Desktop Black Screen (No Monitor Connected)](https://youtu.be/3j4wUMX95zA)

When using remote desktop on Linux, the server (the machine being remoted into) must have a monitor connected.
Otherwise, you will find that although you are connected, the screen is stuck (or black) :sweat:

Here is the solution:

Install `xserver-xorg-video-dummy`:

```cmd
sudo apt-get install xserver-xorg-video-dummy
```

Tested on Ubuntu 18.04 (please install this):

```cmd
sudo apt-get install xserver-xorg-video-dummy-hwe-18.04
```

You can also use the following command to find the corresponding version:

```cmd
sudo apt-cache search video-dummy
```

After installation, please create `xorg.conf` in the path below:

```cmd
sudo vim /usr/share/X11/xorg.conf.d/xorg.conf
```

If you can't find this path, it might also be in `/etc/X11/xorg.conf`.

The content of [xorg.conf](https://github.com/twtrubiks/linux-note/blob/master/enable-ubuntu-remote-tutorial/xorg.conf) is as follows:

```conf
Section "Device"
    Identifier  "Configured Video Device"
    Driver      "dummy"
EndSection
Section "Monitor"
    Identifier  "Configured Monitor"
    HorizSync 31.5-48.5
    VertRefresh 50-70
EndSection
Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
    DefaultDepth 24
    SubSection "Display"
    Depth 24
    Modes "1920x1080"
    EndSubSection
EndSection
```

Then reboot. After that, you will no longer get a black screen when you remote into the server (the machine being remoted into) without a monitor connected :satisfied:

But if you connect a monitor to the server (the machine being remoted into) and boot it up, you will find that the screen does not display anything.
(This is normal, because it is now a dummy screen :smile:)

You just need to delete `xorg.conf` and reboot, and it will be back to normal.
(You can add `xorg.conf` back when you need this function :smile:)

If you want to view the vino settings through the command line:

```cmd
gsettings list-recursively org.gnome.Vino
```

If you want to change the parameters in the settings through the command line:

```cmd
gsettings set org.gnome.Vino require-encryption false
```

If you want to start it through the command line:

```cmd
export DISPLAY=:0 && /usr/lib/vino/vino-server
```

## Conclusion

The above is for the GNOME desktop, but I'm starting to like the KDE desktop more and more.
If you are using the KDE desktop, you can use krfb (Server side) and krdc (Client side).
This is specifically for KDE users, but if you prefer Remmina on the client side, it can also be used normally on KDE :smile:
