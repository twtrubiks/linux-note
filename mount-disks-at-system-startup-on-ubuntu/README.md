# Mount Disk at System Startup On Ubuntu

[Youtube Tutorial - How Mount Disk at System Startup On Ubuntu](https://youtu.be/tYA8HO4jpM4)

When you use Ubuntu(Linux), if you have multiple hard disks,

you will find that other than the system disk, no other hard disk

will be automatically mounted.

Solution.

Open Disks

![alt tag](https://i.imgur.com/DIe9354.png)

select `Edit Mount Options`

![alt tag](https://i.imgur.com/sYhJ61A.png)

Check `Mount at system startup` option

![alt tag](https://i.imgur.com/8vbp6du.png)

Reboot, the hard disks will be mounted automatically at system startup:smile:

In general, you need to mount disk to be correct in `/media`(`/mnt` is a temporary mount point).

`/ media`

Removable mount point, usually it will automatically mount additional hard disk or usb to this file.

`/ mnt`

Used in temporary mounts, usually under this directory is empty. (disappear after reboot)