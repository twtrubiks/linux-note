[中文版](README.md)

## Pros and Cons of Windows -> Linux

[Youtube Tutorial - Windows -> Linux Pros and Cons](https://youtu.be/wCQv7rfv69Q)

It's been 4 months since I switched from Windows to Linux (Ubuntu 18.04 for daily tasks).

I also have a VM environment with Debian 10 (KDE) for development. Debian is incredibly stable :satisfied:

Here's a share of the pros and cons and my experience of switching from Windows to Linux :smile:

### First, the advantages of switching to Linux

1.  **Extremely high freedom (customization)**, especially the DE, which is super fun to play with :smile:
    You can replace any software you like/dislike with others, and set shortcuts however you want.
    It feels like you have complete control over the system, you can do whatever you want, with extremely high freedom.
    Further reading: [Linux Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de)

2.  **Linux uses significantly fewer resources than Windows**, especially in an environment with 8GB of RAM (that cannot be expanded).
    Even though I use GNOME, a more resource-intensive DE, it still feels lighter than Windows and has more resources available.
    If you have an older computer (laptop), you can consider installing Linux (a lighter distro).
    Further reading: [Understanding Linux Distributions](https://github.com/twtrubiks/linux-note/tree/master/linux-distro)

3.  **Linux is stable and reliable**, and it doesn't force you to update the system. Especially Windows, which forces automatic updates. And with Linux updates,
    unless it's a major kernel update, you basically don't need to reboot after updating. This means your computer doesn't need to be turned off at all,
    and you can get to work right away anytime, anywhere. Unlike Windows, where you might find it has rebooted when you get back to your seat :sweat:
    But to be green, it's still recommended to shut down when not in use for a long time :smile:

4.  **Linux can be installed anywhere**, especially on servers, where stability and reliability are the top priorities :smirk:

5.  **Linux is free and open-source**, which means you almost never have to pay. And because it's open-source, more people may review the code,
    which might make it relatively safer than Windows (which is closed-source).

6.  **Linux doesn't require antivirus software** (I only used the built-in antivirus on Windows before), but that doesn't mean Linux can't get viruses
    (although it hasn't happened to me yet XD), and Linux should be safer than Windows.

7.  **For software engineers, many things become simpler** :smile: Some things that are very simple on Linux are
    super weird on Windows :angry: like installing k8s, or installing Docker on Windows (which requires the professional version).
    Sometimes it can be very frustrating :disappointed:

8.  **Linux (Ubuntu) has LTS**, which means long-term support. If you don't have special needs, you can use it stably for 5 years.

9.  **The terminal on Linux (zsh+tmux) is much better to use.**
    Further reading: [How to install Oh My ZSH + tmux on Ubuntu](https://github.com/twtrubiks/Linux-note/tree/master/zsh-tmux-tutorual)

10. **Linux doesn't need defragmentation.** I believe everyone has heard of disk defragmentation on Windows. This is needed because
    Windows uses NTFS, while most Linux systems now use ext4 (which doesn't need defragmentation).

11. **Many useful tools are only available on Linux** :satisfied:

12. **You don't have to worry about not being able to find answers to problems**, because the Linux community is very active :satisfied:

13. **Linux makes computers more interesting** :laughing: It turns using a computer into a learning experience.

### Disadvantages of switching to Linux

1.  **People who need Chinese input methods will find that the Chinese input methods on Linux are not as good** (spoiled by Windows :sweat_smile:)
    Further reading: [A Better Chinese Input Method in Linux That's More Like Microsoft's: Hime](https://github.com/twtrubiks/Linux-note/tree/master/hime-tutorial)

2.  **Playing certain games becomes a bit more troublesome.** It usually requires extra setup (but for me, switching to Linux was to quit gaming) :stuck_out_tongue_closed_eyes:
    Of course, there are also some distros that are more suitable for gaming. If you play games on Steam, it should be completely fine on Linux :smile:

3.  **If you often need to use financial cards, like for online ATMs, many banks don't support Linux**, which is a bit troublesome :persevere:

4.  **Drivers sometimes need to be downloaded separately.** This is because the way drivers are installed on Linux is different from Windows. Windows will install everything for you at once,
    but LINUX already supports a lot, like the `linux-firmware` package.

5.  **If you are a heavy user of Microsoft Office, it might be a bit troublesome.** On Linux, you can only use LibreOffice or the online version.

6.  **This shouldn't be considered a disadvantage, more of a small complaint** :joy: For remote machines, I have used AnyDesk, TeamViewer, VNC Server-RealVNC
    (all for personal use). But it seems that if you install VNC Server-RealVNC on Linux, you have to pay? (It's free on Windows). But it doesn't matter,
    because there are many remote software options for both the server and client side on Linux, but most of them have a limitation, which is that they must be on the internal network (or have a fixed IP).
