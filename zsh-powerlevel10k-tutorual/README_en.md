[中文版](README.md)

# zsh with powerlevel10k

[Youtube Tutorial - zsh-powerlevel10k Tutorial - A Beautiful Terminal](https://youtu.be/5f8w7gYnnfU)

Official documentation: [https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

First, here's a picture:



This example uses Oh My Zsh. If you haven't installed it yet, please refer to the previous article:

[How to install Oh My ZSH](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual)

## Tutorial

Before starting the installation, it is strongly recommended to install the font first (otherwise some icons may not be displayed correctly :sweat:).

Installation process: [https://github.com/romkatv/powerlevel10k#get-started](https://github.com/romkatv/powerlevel10k#get-started)

## Install Font

[the recommended font](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)

[how-was-the-recommended-font-created](https://github.com/romkatv/powerlevel10k#how-was-the-recommended-font-created)

Build the font yourself (using Docker build, it took me a while to clone and build :sweat_smile:).

Or download the ready-made one directly: [Manual font installation](https://github.com/romkatv/powerlevel10k#manual-font-installation)

The official recommendation is to use the MesloLGS NF font.

```cmd
git clone --depth=1 https://github.com/romkatv/nerd-fonts.git
cd nerd-fonts
./build 'Meslo/S/*'
```

If everything is fine, the ttf file will be in `./out`.

![alt tag](https://i.imgur.com/puW0EkH.png)

To install the font:

Copy your font to `/usr/share/fonts/truetype/`.

Then run `sudo fc-cache -f -v` (to update the font cache).

Set the vscode terminal font:

```json
{
    ......
"terminal.integrated.fontFamily": "MesloLGS NF"
    ......
}
```

Set the GNOME terminal font:

![alt tag](https://i.imgur.com/OTKTiXC.png)

Set the Konsole font:

![alt tag](https://i.imgur.com/lvD5QVE.png)

## Install Powerlevel10k

Refer to [https://github.com/romkatv/powerlevel10k#oh-my-zsh](https://github.com/romkatv/powerlevel10k#oh-my-zsh)

```cmd
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Next, run `vim ~/.zshrc` and modify the theme to `ZSH_THEME="powerlevel10k/powerlevel10k"`.

Reopen the terminal and the configuration interface will pop up.

![alt tag](https://i.imgur.com/8KiD5od.png)

If the configuration interface does not pop up automatically, you can manually run `p10k configure`.

## p10k Setting: Enable Transient Prompt

Let me explain this setting.

![alt tag](https://i.imgur.com/3irp0A6.png)

If you choose no, it will be the same as usual.

![alt tag](https://i.imgur.com/A547n6g.png)

If you choose yes, the command you enter will always be at the bottom.

![alt tag](https://i.imgur.com/JrNqbKs.png)