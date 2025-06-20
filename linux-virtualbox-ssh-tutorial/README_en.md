[中文版](README.md)

# Setting up VirtualBox for SSH on Linux

[Youtube Tutorial - Setting up VirtualBox for SSH on Linux](https://youtu.be/Tq_iWo1IIkg)

## Tutorial

Before we begin, please make sure you have `openssh-server` installed.

```cmd
sudo apt-get install openssh-server
```

After installation, it is recommended to test with `ssh localhost`. If you can connect, it means the installation was successful.

![alt tag](https://i.imgur.com/nYo5NNn.png)

## Configuration

First, find the VM you want to connect to -> in Network settings, set "Attached to" to `Bridged Adapter`.

![alt tag](https://i.imgur.com/FPLJtfS.png)

(If the host is Windows, the method is different and more complicated! You will need to set it to NAT and configure Port Forwarding.)

Next, check the IP in the VM.

>> ip addr show

![alt tag](https://i.imgur.com/Xgu6SoD.png)

The IP in the VM is `192.168.1.106`.

Go back to the host machine and enter `ssh twtrubiks@192.168.1.106`.

![alt tag](https://i.imgur.com/NGJCIjo.png)

Successfully connected to the machine in the VM.

![alt tag](https://i.imgur.com/E2OYjLq.png)
