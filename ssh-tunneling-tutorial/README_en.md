[中文版](README.md)

# SSH Tunneling Tutorial

[Youtube Tutorial - SSH Tunneling Tutorial - Local Port Forwarding, Remote Port Forwarding](https://youtu.be/9bsvYo1a-mk)

## Command Description

`-N` means do not execute any command.

```text
Do not execute a remote command.
This is useful for just forwarding ports.
```

`-f` means to run in the background.

```text
Requests ssh to go to background just before command execution.
```

`-L` represents local port forwarding.

`-R` represents remote port forwarding.

For detailed command descriptions, you can view them by running `man ssh`.

## Local Port Forwarding

Suppose there is a remote server A `root@123.123.123.123`.

On remote server A, there is a service on port 5601, which is `http://123.123.123.123:5601`.

Execute the following command on the local machine:

```cmd
ssh -N -L 3000:localhost:5601 root@123.123.123.123
```

(You can ignore it after execution)

Then, browse `http://127.0.0.1:3000/` on your local machine, which is equivalent to connecting to `http://123.123.123.123:5601`.

This is because it forwards the data from local port 3000 to port 5601 on remote server A.

## Remote Port Forwarding

### Scenario One

Suppose there is a remote server A `root@123.123.123.123`.

The IP of the company's internal network computer is `192.168.7.104`.

How can you access the company's internal network computer from home (outside) through remote server A? :question:

As a jump host, execute the following on the company computer:

```cmd
ssh -N -R 8007:192.168.7.104:22 root@123.123.123.123
```

(You can ignore it after execution)

Then, when we are outside (or at home),

first ssh into remote server A `root@123.123.123.123`,

then enter the following command:

```cmd
ssh admin@localhost -p 8007
```

Here, we specify port 8007, which will help us access `192.168.7.104:22`, because the traffic on port 8007 is forwarded to `192.168.7.104:22`.

This way, you can successfully access the company's internal network computer from home (outside) through remote server A.

But be aware that there may still be security issues :exclamation: :exclamation:

### Scenario Two

Suppose there is a remote server A `root@123.123.123.123`, and it has a public URL, say `www.example.com`.

And on my local machine, there is a service on port 8069, `http://127.0.0.1:8069/`.

I want customers to be able to connect to the service on my local machine's port 8069 (i.e., `http://127.0.0.1:8069/`) through the URL `www.example.com`.

Tutorial:

First, install nginx on your remote server A and set up `proxy_pass`.

```cmd
vim /etc/nginx/sites-available/default
```

The content is roughly as follows:

```text
...

location / {
    proxy_pass http://localhost:8002/;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #proxy_set_header X-Forwarded-Proto https;
}
...
```

Some nginx commands:

```cmd
# Restart nginx
sudo systemctl restart nginx

# View logs
cat /var/log/nginx/access.log
```

Then execute the following on the local machine:

```cmd
ssh -N -R 8002:localhost:8069 root@123.123.123.123
```

Accessing `www.example.com` is equivalent to accessing `http://127.0.0.1:8069/` on the company computer.

This logic is the exact opposite of Local Port Forwarding.

Port 8002 is just for us to listen on, and through this port, the connection between the two sides is completed.

## Reference

* [Reverse SSH Tunneling - From Start to End](https://jfrog.com/connect/post/reverse-ssh-tunneling-from-start-to-end/)
* [SSH Tunneling (Port Forwarding) Explained in Detail](https://johnliu55.tw/ssh-tunnel.html)
* [DIY ngrok: Build a Poor Man's ngrok Service](https://5xruby.tw/posts/easy-ngrok-by-nginx-ssh-tunnel)
