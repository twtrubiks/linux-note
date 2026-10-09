[中文版](README.md)

# GNU screen — Keep Programs Running in the Background After Exiting SSH

`screen` is a terminal multiplexer. Its most common use is:

**Running a program on a remote server and keeping it running in the background after you exit SSH**.

A similar tool is tmux, see [zsh-tmux-tutorual](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual).

## How It Works

```
Your computer ──SSH──▶ Remote server
                       └─ screen session (lives on the server)
                            └─ Your program (runs inside screen)
```

- **Without screen**: when SSH disconnects, the shell receives a hangup signal, and the programs under it get killed along with it.
- **With screen**: the program belongs to the screen session and has nothing to do with this SSH connection, so after you detach, leaving SSH with `exit` won't affect it.

What the official manual says:

- Programs keep running when *"the whole screen session is detached from the user's terminal"* (Section 1 Overview)
- Detach means *"disconnect it from the terminal and put it into the background"* (Section 8.1 Detach)

### No Worries About Disconnects: autodetach

Section 8.1 of the manual says *"Autodetach is on by default"*. Even if you didn't press `Ctrl+A, D` and the network just drops, screen will automatically detach and keep the programs inside alive.

## Installation

screen needs to be installed **on the remote server** (not on your computer), and the program also runs inside the screen session on the server.

```cmd
sudo apt install screen
```

## Basic Workflow

```bash
# 1. SSH into the remote server
ssh user@your-server

# 2. Start a named screen session
screen -S mywork

# 3. Run your program inside it
python long_task.py

# 4. Detach from screen (the program keeps running)
#    Ctrl+A, D

# 5. Leave SSH
exit

# --- Later, to reattach ---
ssh user@your-server
screen -r mywork
```

## Ctrl+A, D Is Detach, Not exit

This is the part people mix up the most :exclamation:

A lot of people can't get the `Ctrl+A, D` key combo to work. It's actually just two steps:

1. `Ctrl+A` — hold Ctrl and A at the same time, then **release**
2. `D` — then press D **on its own**

| Action | Result |
|------|------|
| `Ctrl+A, D` | ✅ detach, leaves screen, the program keeps running |
| Typing `exit` directly **inside** screen | ❌ closes the shell, the window closes with it, and the program ends too |

The manual's Section 1 Overview puts it as *"When a program terminates, screen (per default) kills the window that contained it ... if none are left, screen exits."*, which means once the shell in the last window runs `exit`, the whole screen session ends.

So **to leave screen while keeping the program running, use `Ctrl+A, D`, not `exit`**.

## Common Commands

| Command | Description |
|------|------|
| `screen -S <name>` | Start a new session with a name |
| `screen -ls` | List all sessions |
| `screen -r <name>` | Reattach a detached session |
| `screen -d -r <name>` | If the session is still attached somewhere else, detach it first, then reattach |
| `screen -x <name>` | Attach to a session that is already attached somewhere else (multiple terminals viewing the same screen at once) |
| `screen -X -S <name> quit` | Terminate a session directly from outside (no need to attach to it) |

### What to Do When screen -ls Shows Attached

The manual's Section 3 description of `-ls`: sessions marked detached can be resumed with `screen -r`, while sessions marked attached still have a terminal connected.

And `-r` is *"Resume a detached screen session"*, so when a session shows `(Attached)` (e.g. the old SSH connection hasn't timed out yet), `screen -r` can't reattach to it. In that case use

```cmd
screen -d -r <name>
```

The manual describes `-d -r` as *"Reattach a session and if necessary detach it first"*.

### Deleting Sessions

Delete a specific session

```bash
# Method 1: attach to it first, then exit
screen -r mywork
exit

# Method 2: terminate it directly from outside (no need to attach)
screen -X -S mywork quit
```

Delete all sessions

```bash
screen -ls | grep -oP '\d+\.\S+' | xargs -I {} screen -X -S {} quit
```

## Keyboard Shortcuts Inside screen

Every shortcut starts with pressing `Ctrl+A`, releasing it, then pressing the next key.

| Shortcut | Description |
|--------|------|
| `Ctrl+A, D` | **detach**, leave screen, the program keeps running |
| `Ctrl+A, C` | Open a new window (new shell) |
| `Ctrl+A, N` / `Ctrl+A, P` | Switch to the next / previous window |
| `Ctrl+A, 0~9` | Switch to the window with that number |
| `Ctrl+A, "` | List all windows to pick from |
| `Ctrl+A, Shift+A` | Name the current window |
| `Ctrl+A, K` | Kill the current window |
| `Ctrl+A, [` | Enter scrollback / copy mode (lets you look back at output) |
| `Ctrl+A, ?` | Show all shortcuts |
| `Ctrl+A, A` | Send an actual `Ctrl+A` to the program (e.g. bash's "move cursor to beginning of line") |

To leave scrollback mode, just press `Esc`. The manual's Section 12.1.8 Specials says *"All keys not described here exit copy mode."*, and since `Esc` isn't in the scrollback mode key list, it exits right away.

Note :exclamation: screen's prefix key is `Ctrl+A`, which happens to conflict with bash / zsh's "move cursor to beginning of line". Inside screen, press `Ctrl+A, A` instead to move to the beginning of the line.

tmux's prefix key is `Ctrl+B`, so it doesn't have this problem.

## Notes

| Situation | Result |
|------|------|
| Leave SSH with `exit` after `Ctrl+A, D` | ✅ the program keeps running |
| SSH suddenly disconnects | ✅ the program keeps running (autodetach) |
| Typing `exit` directly **inside** screen | ❌ the whole screen session (or that window) closes, and the program ends too |
| The remote server reboots | ❌ the screen session disappears and has to be started again |

The server reboot row is an inference, not from the manual itself: the screen session is just a regular process on the server, so a reboot will definitely terminate it.

## Practical Use - Claude Code Remote Control

Run Claude Code's remote control inside screen on a remote server, then keep controlling it from your phone after exiting SSH.

```bash
# Step 1: SSH into the remote server
ssh user@your-server

# Step 2: Start a screen session
screen -S claude

# Step 3: Start claude code's remote control
claude remote-control

# Step 4: Connect from your phone (see the official docs for how)

# Step 5: Safely detach from screen (won't close Claude)
#         Ctrl+A, D

# Step 6: Exit SSH
exit
```

Later, to reattach from your computer

```bash
ssh user@your-server
screen -r claude
```

For Remote Control details, see the [Claude Code Remote Control official docs](https://code.claude.com/docs/en/remote-control).

## References

- [GNU Screen official manual](https://www.gnu.org/software/screen/manual/screen.html)
  - Section 1 Overview: programs keep running after detach, a program exiting closes its window
  - Section 3 Invoking Screen: `-S`, `-r`, `-ls`, `-x`, `-d -r`, `-X` options
  - Section 5.1 Default Key Bindings: keyboard shortcuts
  - Section 8.1 Detach: detach and autodetach
  - Section 12.1 Copying: scrollback / copy mode
- [Debian screen package](https://packages.debian.org/search?keywords=screen&exact=1)
