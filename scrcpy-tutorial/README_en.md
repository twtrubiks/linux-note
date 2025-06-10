[中文版](README.md)

# Linux Ubuntu Tutorial - Control (Project) Your Phone with Your Computer Using scrcpy

[Youtube Tutorial - Linux Ubuntu Tutorial - Control (Project) Your Phone with Your Computer Using scrcpy](https://youtu.be/4mwrO3p1xHk)

Control your phone with your computer using [scrcpy](https://github.com/Genymobile/scrcpy).

## Installation Tutorial

This demonstration uses Ubuntu 18.04.

If you are on Ubuntu 20.04, you can simply use:

```cmd
apt install scrcpy
```

Or use `snap` for a straightforward installation.
(But I don't like snap, so building it myself is the only way to go.)

It is recommended to follow along with the [build the app manually](https://github.com/Genymobile/scrcpy/blob/master/BUILD.md) guide.

### Debian/Ubuntu - Runtime Dependencies

```cmd
sudo apt install ffmpeg libsdl2-2.0-0 adb
```

### Client Build Dependencies

```cmd
sudo apt install gcc git pkg-config meson ninja-build \
                 libavcodec-dev libavformat-dev libavutil-dev \
                 libsdl2-dev
```

### Server Build Dependencies

```cmd
sudo apt install openjdk-8-jdk
```

When you run the following command, you will definitely encounter an issue with `meson` being too old.
So, please execute the following commands:

```cmd
sudo apt install python3-pip
pip3 install meson
```

A reminder here, the path for pip3 installation is `~/.local/bin/meson`.

To ensure you have the correct version, please go to [scrcpy/releases](https://github.com/Genymobile/scrcpy/releases) and download the latest version.

You need to download two things: the Source code and `scrcpy-server-vx.xx`.

Unzip the Source code, rename the folder to `scrcpy`, then rename `scrcpy-server-vx.xx` to `scrcpy-server` and move it into the `scrcpy` folder.
(Renaming is necessary to avoid compilation errors.)

Next, `cd` into the `scrcpy` directory.

### Build

```cmd
~/.local/bin/meson x --buildtype release --strip -Db_lto=true -Dprebuilt_server=scrcpy-server
```

(It is recommended to use an absolute path for `~/.local/bin/meson`.)

The following command is prone to errors (be sure to follow my previous instructions, or it will likely fail):

```cmd
ninja -Cx
```

If there are no errors up to this point, execute the final command:

```cmd
sudo ninja -Cx install
```

And you're done.

Before using scrcpy, it is recommended to use `adb devices` to confirm that your phone is connected.
Note: Please enable debugging on your phone.



Then, simply run `scrcpy`.
