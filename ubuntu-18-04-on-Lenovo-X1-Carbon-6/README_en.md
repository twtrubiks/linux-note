[中文版](README.md)

# Ubuntu 18.04 on Lenovo X1 Carbon 6

[Youtube Tutorial - Lenovo X1 Carbon 6 (from win10 to ubuntu18.04) Experience & Notes](https://youtu.be/okyAzo-urcw)

Reinstalling Linux from Windows on an X1C6.

Reasons:

1.  I really don't like Hyper-V on Windows :cry:
2.  The RAM is only 8GB and is soldered on, which is too resource-intensive on Windows.
3.  The next milestone for a software engineer :satisfied:

Please refer to the following article directly:

[ubuntu-18-04-on-lenovo-x1-carbon-6g](https://medium.com/@hkdb/ubuntu-18-04-on-lenovo-x1-carbon-6g-d99d5667d4d5)

My own laptop is also an X1C 6 (it was really expensive when I bought it).

My specs are as follows:

![alt tag](https://i.imgur.com/iMQ1pnY.png)

Basically, it has 4 CPUs, but I don't know why it shows 8 CPUs after installing Ubuntu :smile:

The X1C 6 series has very good support for Linux. Basically, I have no driver issues at all.

The only thing I haven't tested is the fingerprint reader, because I don't think it's useful and I don't need it :grin:

The only thing to note when switching from Windows to Linux is that you need to go into the BIOS to change the settings.

You need to change to the Linux system's s3 Linux sleep (otherwise the system will consume a lot of power in standby mode :angry:).

If you can't find this option in the BIOS, it means your BIOS version is too old. Updating the BIOS will make this option appear. (I couldn't find it at first either, but it appeared after updating the BIOS).

There are also many tweaks in the article, which you can configure as you see fit.

I followed many of the settings, and now my laptop is very power-efficient.

Windows is really too resource-intensive and power-consuming :joy:
