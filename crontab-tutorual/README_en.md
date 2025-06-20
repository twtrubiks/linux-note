[中文版](README.md)

# Crontab

[Youtube Tutorial - Linux Command Tutorial - Crontab](https://youtu.be/4mzBFM8bXbw)

Show current scheduled tasks:

```cmd
crontab -l
```

Edit scheduled tasks (enter modification mode):

```cmd
crontab -e
```

Remove scheduled tasks (deletes all of the user's crontab entries):

```cmd
crontab -r
```

Let's try it once. For example, `hello.py` is as follows:

```python
#!/usr/bin/python3
import datetime

with open('dateInfo.txt','a') as outFile:
    outFile.write('\n' + str(datetime.datetime.now()))
```

First, run `crontab -e`.

It will ask you which editor to use. You can choose one you like.
If you want to change it later, you can use `select-editor` to modify it.

After selecting, a screen will pop up.
This is where you set up your crontab. Enter the following:

```shell
# Run hello.py every minute
* * * * *  /usr/bin/python3 /home/twtrubiks/hello.py
```

![alt tag](https://i.imgur.com/YAHgSTd.png)

Then start the service:

```cmd
service cron start
```

Then, every minute, you will find that new data has been written to `dateInfo.txt`.

Stop the service:

```cmd
service cron stop
```

Restart the service:

```cmd
service cron restart
```

Reload the configuration:

```cmd
service cron reload
```

Check the status of the crontab service:

```cmd
service cron status
```

![alt tag](https://i.imgur.com/yuakltL.png)

Time setting description:

Run every minute:

```cmd
* * * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run every 5 minutes:

```cmd
*/5 * * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run every hour:

```cmd
0 * * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run at 1:00 AM every day:

```cmd
0 1 * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run once a day from the 1st to the 20th of the month:

```cmd
0 1 1-20 * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run every 5 minutes from 10 AM to 3 PM:

```cmd
*/5 10-15 * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

Run every 30 minutes between 9 AM and 2 PM from Monday to Friday:

```cmd
*/30 9-14 * * Mon-FRI
```

Sometimes, to verify the crontab, you can also write the executed file or log out:

```cmd
* * * * *  /usr/bin/python3  /home/twtrubiks/hello.py > /tmp/test_check.txt
```

`*` represents any time.

```cmd
* * * * *
minute hour dayOfMonth month dayOfWeek
```

`,` separates time periods. For example, `30 10,18 * * * command` means to execute the command at 10:30 AM and 6:30 PM.

`-` represents a range of time. For example, `15 10-12 * * * command` means to execute the command at the 15th minute of **every** hour from 10 AM to 12 PM.

`/n`, where n is a number, represents every n unit interval. For example, `*/5 * * * * command` means to execute the command every 5 minutes.

Here is a table for everyone:

| * | * | * | * | * |
|:---:|:---:|:---:|:---:|:---:|
| Minute | Hour | Day of Month | Month | Day of Week |
| 0 to 59 | 0 to 23 | 1 to 31 | 1 to 12 | 0 to 7 (0 and 7 mean Sunday, 1 means Monday, 2 means Tuesday) |

You can use this with the website [https://crontab.guru/](https://crontab.guru/).

Actually, there is an even simpler method: use [python-crontab](https://pypi.org/project/python-crontab/) to control crontab directly.
