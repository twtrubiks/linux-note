[中文版](README.md)

# Rclone tutorial

[Youtube Tutorial - Rclone tutorial - Awesome Cloud Sync Tool](https://youtu.be/0ChhvaHIQ9Y)

rclone is a great file synchronization management tool that can sync with many cloud services, such as Dropbox, Google Drive, and more.

Please refer to the official website for instructions: [rclone](https://rclone.org/).

## Tutorial

For installation instructions, see the official documentation: [rclone.org-install](https://rclone.org/install/)

Since some dependencies may be required, it is recommended to install them first:

```cmd
sudo apt-get update
sudo apt-get install curl unzip
```

Install rclone:

```cmd
curl https://rclone.org/install.sh | sudo bash
```

Verify that the version was installed successfully:

```cmd
sudo rclone --version
```

Start configuration:

```cmd
sudo rclone config
```



We will use Dropbox as a demonstration, which is number 9.



For `client_id` and `client_secret`, just press enter (leave them blank).

For `Edit advanced config`, choose `n`.



For `Auto config`, choose `y`.

If the URL does not open automatically, please open it manually.



Enter your Dropbox account information.







If the information is correct, please choose `y`.



You can use the following command to confirm the remotes:

```cmd
sudo rclone listremotes
```

Note that it is `dropbox:`, with a colon `:`.



Using the [rclone_sync](https://rclone.org/commands/rclone_sync/) command.

Sync from local to remote:

```cmd
sudo rclone sync -v ~/linux-note dropbox:temp
```

Note: -v, -vv, --verbose



Sync from remote back to local:

```cmd
sudo rclone sync -v dropbox:temp ~/test-data
```


List directories on the remote:

[rclone_lsd](https://rclone.org/commands/rclone_lsd/)

```cmd
sudo rclone lsd dropbox:
```

## Other commands

Refer to [rclone-docs](https://rclone.org/docs/).

List all files on the remote:

[rclone_ls](https://rclone.org/commands/rclone_ls/)

```cmd
sudo rclone ls dropbox:
```

List directories on the remote:

[rclone_lsd](https://rclone.org/commands/rclone_lsd/)

```cmd
sudo rclone lsd dropbox:
```

Copy files, skipping those that have already been copied (exist).

[rclone_copy](https://rclone.org/commands/rclone_copy/)

You can also move files between cloud services, like in the following example:

```cmd
sudo rclone copy --drive-chunk-size=64M --transfers=2 --fast-list --stats=20s -v remote_1:folder remote_2:folder
```

[rclone_move](https://rclone.org/commands/rclone_move/)

```cmd
sudo rclone move remote_1:folder remote_2:folder
```

For detailed differences between `sync`, `copy`, and `move`, please read the documentation yourself. :smile:

You can also use it with [crontab-tutorial](https://github.com/twtrubiks/linux-note/tree/master/crontab-tutorual) to solve scheduled synchronization problems. :satisfied:

## Cloud Encryption

rclone also has an encryption feature. For example, if I don't have much confidence in cloud provider A's security, I would want my uploaded files to be encrypted. You can use this option for that.



Then select the remote you created, like `dropbox:` which I created earlier. You can name it something like `dropbox:Encrypt`. In short, you must first create a remote, and then you can create an encrypted one. If anyone is interested in this part, I will make a video to teach you later. :smile:
