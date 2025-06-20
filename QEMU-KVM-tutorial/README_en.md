# QEMU-KVM Tutorial

[中文版](README.md)

[Youtube Tutorial - Linux QEMU-KVM Tutorial](https://youtu.be/702H2gkJzCE)

To get straight to the point, theoretically, compared to VirtualBox, using KVM (Kernel-based Virtual Machine) is faster :smile:

## Prerequisites

First, check if your computer supports KVM.

```cmd
sudo apt update
sudo apt install cpu-checker
```

Then run `kvm-ok`.

If it's supported, it will display:

```text
INFO: /dev/kvm exists
KVM acceleration can be used
```

![alt tag](https://i.imgur.com/kfZtalI.png)

You can then perform a further check.

For INTEL CPUs, enter the following command:

```cmd
grep -c vmx /proc/cpuinfo
```

![alt tag](https://i.imgur.com/0zZXUeq.png)

For AMD CPUs, enter the following command:

```cmd
grep -csvm /proc/cpuinfo
```

Normally, this should display the number of available CPUs.

Also, this feature might be disabled in some BIOS settings. Please go to Security in your BIOS and enable Virtualization Tech and VT-D.

## Installation Command

```cmd
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

After installation, check if the libvirtd service is running.

```cmd
sudo systemctl status libvirtd.service
```

![alt tag](https://i.imgur.com/ZwR3mcj.png)

Set permissions to access VMs (optional).

If you don't do this, you might be asked for your user password when opening QEMU.

```cmd
sudo adduser $USER libvirt

sudo adduser $USER libvirt-qemu
```

It's recommended to reboot after installation.

## Create a New VM

Choose to create from an ISO.

![alt tag](https://i.imgur.com/WarAGID.png)

Select your ISO file.

For the operating system below, if you can't find yours, select Generic default.

![alt tag](https://i.imgur.com/sSjEahT.png)

Set your CPU and RAM.

![alt tag](https://i.imgur.com/AyH4XEb.png)

Set the size of the VM.

![alt tag](https://i.imgur.com/EgDLtCK.png)

Customize configuration means you can manually set up network, USB, etc., before the first boot.

However, this can all be modified later.

![alt tag](https://i.imgur.com/DvmFhrP.png)

The setup is now complete, and you can start the VM directly.

All VM settings can be found in the "i" icon.

If you want to add other things (network, USB...), you can click "Add Hardware" at the bottom.

![alt tag](https://i.imgur.com/0nfo7Ep.png)

## QEMU-KVM Full Screen

You can set this in View -> Scale Display.

![alt tag](https://i.imgur.com/YnClVIA.png)

If it's still not full screen after setting, please adjust the resolution within the VM.

## QEMU-KVM SSH Setup

Usually, the IP address will be here (but for some reason, it's not showing here, maybe because it's Windows :expressionless:)

![alt tag](https://i.imgur.com/3t100CQ.png)

Remember to install openssh-server in the VM.

`sudo apt-get install openssh-server`

The concept is the same as VirtualBox, you can refer to [Setup VirtualBox for SSH on Linux](https://github.com/twtrubiks/linux-note/tree/master/linux-virtualbox-ssh-tutorial)

## QEMU-KVM copy&paste from the host

Those who frequently use VirtualBox know there's a feature for bidirectional copy (text) and paste between the host and the virtual machine, which can be very convenient :smile:

If your virtual machine is Linux, execute the following command:

```cmd
sudo apt install spice-vdagent
```

Please remember to reboot :smile:

If your virtual machine is Windows, please install the following software:

[https://www.spice-space.org/download/binaries/spice-guest-tools/](https://www.spice-space.org/download/binaries/spice-guest-tools/)

Theoretically, it should take effect automatically after installation. If not, please restart your virtual machine.

## Convert VirtualBox Image to QEMU Image

Usually, if you've used QEMU, you'll want to switch to it because it's much faster :smile:

Here's how to convert the image.

Use the following command to convert:

```cmd
VBoxManage clonehd --format RAW <vdisk-name>.vdi <vdisk-name>.img
```

![alt tag](https://i.imgur.com/7kFph4Y.png)

```cmd
qemu-img convert -f raw <vdisk-name>.img -O qcow2 <vdisk-name>.qcow2
```

![alt tag](https://i.imgur.com/KamBcuN.png)

Choose to import an existing image.

![alt tag](https://i.imgur.com/YAPN52T.png)

Select the qcow2 file you just exported (you can place the qcow2 file under `/var/lib/libvirt/images/`).

![alt tag](https://i.imgur.com/EVLhKmC.png)

![alt tag](https://i.imgur.com/8VWKthz.png)

![alt tag](https://i.imgur.com/SWqjQLN.png)

After the above settings are complete, you can start it directly.

Successful conversion.

![alt tag](https://i.imgur.com/ntiE9if.png)