[中文版](README.md)

# Linux inotify-tools: Monitor File Changes with inotifywait

* [Youtube Tutorial - Linux Tutorial - inotify-tools: Monitor File Changes with inotifywait](https://youtu.be/BsQqH40eOKY)

`inotifywait` is a tool under `inotify-tools`.

It is mainly used to monitor any changes (add, delete, modify, etc.) to files or directories.

You can use it to perform tasks like automatic backup uploads or automatic reloading of configuration files.

The installation command is as follows:

```cmd
sudo apt install inotify-tools
```

If you want to see the documentation for `inotifywait`, you can enter the following command:

```cmd
inotifywait --help
```

or

```cmd
man inotifywait
```

## Monitor All File Changes in a Specific Directory

### Monitoring a Directory

```cmd
inotifywait -m /home/temp
```

`-m` `--monitor` keeps listening for events forever. The description is as follows:

```txt
Keep listening for events forever.  Without
this option, inotifywait will exit after one
event is received.
```

If there are subdirectories within the folder, you can add the `-r` flag:

```cmd
inotifywait -m -r /home/temp
```

`-r` `--recursive` Watch directories recursively.

### Monitoring a File

```cmd
inotifywait -m /home/temp/test.py
```

## Custom Output Message Format

You can define the text you want to output.

```cmd
inotifywait -m --timefmt '%Y/%m/%d %H:%M:%S' --format "%T %w %f %e" -e create  /home/temp
```

Here is the `--format` description from `man inotifywait`:

```txt
%w     This will be replaced with the name of the Watched file on which
        an event occurred.

%f     When an event occurs within a directory, this will  be  replaced
        with the name of the File which caused the event to occur.  Oth‐
        erwise, this will be replaced with an empty string.

%e     Replaced with the Event(s) which occurred, comma-separated.

%Xe    Replaced with the Event(s) which occurred, separated  by  which‐
        ever character is in the place of `X'.

%T     Replaced  with  the  current Time in the format specified by the
        --timefmt option, which should be a format string  suitable  for
        passing to strftime(3).
```

## Monitor File Events

Monitor `modify` and `delete` events:

```cmd
inotifywait -m -e modify,delete /home/temp
```

`-e` `--event` description is as follows:

```txt
Listen for specific event(s).  If omitted, all events are listened for.
```

There are many events, as documented below:

```txt
Events:
	access		file or directory contents were read
	modify		file or directory contents were written
	attrib		file or directory attributes changed
	close_write	file or directory closed, after being opened in
	           	writable mode
	close_nowrite	file or directory closed, after being opened in
	           	read-only mode
	close		file or directory closed, regardless of read/write mode
	open		file or directory opened
	moved_to	file or directory moved to watched directory
	moved_from	file or directory moved from watched directory
	move		file or directory moved to or from watched directory
	create		file or directory created within watched directory
	delete		file or directory deleted within watched directory
	delete_self	file or directory was deleted
	unmount		file system containing file or directory unmounted
```

## Write Output Messages to a File

By default, `inotifywait` outputs all messages to stdout.

But you can also output them to a file for preservation.

```cmd
inotifywait -m -o logs.txt /home/temp
```

`-o` `--outfile` Print events to file rather than stdout.

`-s` `--syslog` Send errors to syslog rather than stderr.

## Run in the Background

`inotifywait` can also run in the background by adding the `-d` flag. This is usually used with `-o` or `-s`.

```cmd
inotifywait -m -o events.log -d /home/temp/test.py
```

`-d` `--daemon` description is as follows:

```txt
Same as --monitor, except run in the background

logging events to a file specified by --outfile.

Implies --syslog.
```

If it's running in the background and you want to stop `inotifywait`, you can use the following commands:

First, find the PID, then kill it.

```cmd
ps -ef | grep inotifywait
kill PID
```

Or kill `inotifywait` directly:

```cmd
killall inotifywait
```

## Other Application Examples

### Triggering a Bash Script

```cmd
inotifywait -m -e attrib /home/temp/ |
  while read events; do
    echo "hello world"
  done
```

### Executing a Program

```cmd
inotifywait -m -e close_write /home/temp/ |
  while read events; do
    python3 /home/test.py
  done
```

### Automatically Reloading Configuration Files

Every time you modify `crontab`, you have to manually run `sudo service cron reload`.

Now, you can automate this with `inotifywait`.

For Debian and Ubuntu, the files are stored in `/var/spool/cron/crontabs`.

This means when you modify `crontab -e`, the files in this directory will change.

The command is as follows:

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs |
  while read events; do
    echo <pwd> | sudo service cron reload
  done
```

You can also try running it in the background:

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs \
-d -o /home/logs.txt |
  while read events; do
    echo <pwd> | sudo service cron reload
  done
```

A final note: `inotifywait` will be disabled after a reboot.

So, you can set the above command to run automatically on startup or configure it with `systemctl`.

For `systemctl`, you can refer to the previous articles [systemctl](https://github.com/twtrubiks/linux-note/tree/master/systemctl-tutorial) or [Automatically start Docker in Linux](https://github.com/twtrubiks/docker-tutorial/tree/master/docker-auto-run-linux) :smile:

Create `watch.service`:

```cmd
sudo vim /etc/systemd/system/watch.service
```

Fill `watch.service` with the following:

```sh
[Unit]
Description=my watch
Requires=watch.service
After=watch.service

[Service]
Type=simple
RemainAfterExit=yes
WorkingDirectory=<YOUR PATH>
ExecStart=/usr/bin/sh watch.sh
TimeoutStartSec=10

[Install]
WantedBy=multi-user.target
```

`watch.sh` is as follows (the code introduced earlier):

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs \
-d -o /home/logs.txt |
  while read events; do
    sudo service cron reload
  done
```

Reload the configuration file:

```cmd
sudo systemctl daemon-reload
```

Start `watch.service`:

```cmd
sudo systemctl start watch
sudo systemctl enable watch
