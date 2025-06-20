[中文版](README.md)

# speedtest-cli Tutorial

[Youtube Tutorial - Linux speedtest-cli Tutorial](https://youtu.be/qOJdGSxHj6Q)

Test internet speed using the command line.

## Description

First, install `speedtest-cli`.

```cmd
sudo apt-get install speedtest-cli
```

For command instructions, you can refer to:

```cmd
speedtest-cli --help
```

Using `speedtest-cli` is very simple:

```cmd
speedtest-cli
```

![alt tag](https://i.imgur.com/bloPnpf.png)

There are also other commands available.

To display only basic information and test only the upload speed:

```cmd
speedtest-cli --simple --no-download
