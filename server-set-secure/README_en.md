# Server Security Settings

[中文版](README.md)

This guide references Linode's [Guides - Setting Up and Securing a Compute Instance](https://www.linode.com/docs/products/compute/compute-instances/guides/set-up-and-secure/).

## Configuration

```cmd
sudo vim /etc/ssh/sshd_config
```

The relevant settings are as follows:

```config
Port 8056
PermitRootLogin no
PasswordAuthentication no
```

`Port 8056`: The default is 22. Change it to a port of your choice to reduce the chance of being attacked.

`PermitRootLogin no`: Disable root login.

`PasswordAuthentication no`: Disable password-based login.

After configuring, you must execute the following command for the changes to take effect:

```cmd
sudo service ssh restart
```

## Create a New User

We have already disabled the root user above, so we need to create a new user here.

Add a new user:

```cmd
adduser twtrubiks
```

Add this user to the sudo group:

```cmd
usermod -a -G sudo twtrubiks
```

If you want to change the password later, you can use the following command:

```cmd
passwd twtrubiks
```

After this, the login command will be:

```cmd
ssh twtrubiks@xx.xx.xxx.xx -p 8056
```

## Modify Host Name

```cmd
sudo vim /etc/hostname
```

Simply change the hostname to the name you want.

```cmd
sudo vim /etc/hosts
```

Set it up similar to the following:

```txt
127.0.0.1       localhost
127.0.1.1      twtrubiks
```

You need to reboot for this to take effect.

## Others

In Linode's Network settings, there is a Reverse DNS option. You can change this to match your domain name. The change will take effect in about 4-24 hours. (You can use `ping your_domain` to check if it's effective).

Also, if you use Docker and later configure Linode's firewall, you need to restart your Docker containers for the firewall rules to apply.
