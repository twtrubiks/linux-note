# QEMU-KVM 教學

[Youtube Tutorial - Linux QEMU-KVM 教學](https://youtu.be/702H2gkJzCE)

直接說重點, 理論上, 相比 VirtualBox,

使用 KVM (Kernel-based Virtual Machine) 速度是比較快的:smile:

## 前置作業

先確認自己的電腦是否支援 KVM,

```cmd
sudo apt update
sudo apt install cpu-checker
```

然後執行 `kvm-ok`,

如果支援, 會顯示

```text
INFO: /dev/kvm exists
KVM acceleration can be used
```

![alt tag](https://i.imgur.com/kfZtalI.png)

然後可以再更加一步確認,

INTEL CPU 輸入以下指令

```cmd
grep -c vmx /proc/cpuinfo
```

![alt tag](https://i.imgur.com/0zZXUeq.png)

AMD CPU 輸入以下指令

```cmd
grep -c svm /proc/cpuinfo
```

正常情況應該會顯示出可用 cpu 的數量.

然後有些 BIOS 可能是關閉這項功能的,

請到 BIOS 中的 Security 打開 Virtualization Tech 以及 VT-D.

## 安裝指令

```cmd
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

安裝完確認一下, libvirtd service 是否運作

```cmd
sudo systemctl status libvirtd.service
```

![alt tag](https://i.imgur.com/ZwR3mcj.png)

設定訪問 VM 權限 (也可以不做),

如果不做, 有時候開啟 QEMU 時, 會要你輸入user密碼

```cmd
sudo adduser $USER libvirt

sudo adduser $USER libvirt-qemu
```

安裝完建議重開機.

## 建立一個新的 VM

選擇透過 ISO 建立

![alt tag](https://i.imgur.com/WarAGID.png)

選擇你的 ISO 檔,

下方的作業系統, 如果選不到, 就選 Generic default

![alt tag](https://i.imgur.com/sSjEahT.png)

設定你的 cpu 和 ram

![alt tag](https://i.imgur.com/AyH4XEb.png)

設定 VM 的大小

![alt tag](https://i.imgur.com/EgDLtCK.png)

自訂組態, 代表在第一次啟動前時, 會讓你手動設定 網路 usb 等等,

但這個都可以再修改.

![alt tag](https://i.imgur.com/DvmFhrP.png)

以上這樣就設定完畢了, 可以直接啟動 VM.

所有的 VM 設定都可以在 驚嘆號這個 ICON 裡面找到,

如果你還想要自己加入其他(網路 usb...), 可點選最下面的自己新增.

![alt tag](https://i.imgur.com/0nfo7Ep.png)

## QEMU-KVM 全螢幕

可到 View -> Scale Display 設定

![alt tag](https://i.imgur.com/YnClVIA.png)

如果設定完之後還是沒有全屏, 請到 VM 中調整解析度.

## QEMU-KVM ssh 設定

通常在這邊就會有 ip (但不知道為甚麼這邊沒有顯示, 可能是因為 win:expressionless:)

![alt tag](https://i.imgur.com/3t100CQ.png)

記得 VM 中要安裝 openssh-server

`sudo apt-get install openssh-server`

和 VirtualBox 的概念是一樣的, 可參考 [在 Linux 中設定 VirtualBox 把玩 ssh](https://github.com/twtrubiks/linux-note/tree/master/linux-virtualbox-ssh-tutorial)

## QEMU-KVM copy&paste from the host

常用 VirtualBox 都知道有一個功能是本地端和虛擬機的

雙向複製(文字)貼上, 有時候會方便很多:smile:

如果你的虛擬機是 linux, 請執行以下指令

```cmd
sudo apt install spice-vdagent
```

請記得重新開機:smile:

如果你的虛擬機是 windows, 請安裝以下軟體

[https://www.spice-space.org/download/binaries/spice-guest-tools/](https://www.spice-space.org/download/binaries/spice-guest-tools/)

理論上安裝完就會自動生效了, 如果沒有生效, 請重新啟動你的虛擬機.

## VirtualBox Image 轉換到 QEMU Image

通常如果你使用過 QEMU, 都會想轉換到這邊, 因為速度快不少:smile:

這邊教大家如何轉換 Image,

使用以下指令轉換

```cmd
VBoxManage clonehd --format RAW <vdisk-name>.vdi <vdisk-name>.img
```

![alt tag](https://i.imgur.com/7kFph4Y.png)

```cmd
qemu-img convert -f raw <vdisk-name>.img -O qcow2 <vdisk-name>.qcow2
```

![alt tag](https://i.imgur.com/KamBcuN.png)

選擇匯入既有的 image

![alt tag](https://i.imgur.com/YAPN52T.png)

選擇剛剛導出的 qcow2 (可以把 qcow2 放入 `/var/lib/libvirt/images/` 底下)

![alt tag](https://i.imgur.com/EVLhKmC.png)

![alt tag](https://i.imgur.com/8VWKthz.png)

![alt tag](https://i.imgur.com/SWqjQLN.png)

以上設定完畢, 直接啟動即可.

成功轉換,

![alt tag](https://i.imgur.com/ntiE9if.png)
