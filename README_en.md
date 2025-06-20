[中文版](README.md)

# linux-note

This is mainly a record of some Linux commands 📝

(This article will be continuously updated :smile:)

## cd

Change to the home directory `~`

```cmd
cd ~
```

Change to the root directory `/`

```cmd
cd /
```

Go back to the parent directory

```cmd
cd ..
```

Move to the previous directory (very convenient for quickly switching between two paths :smile:)

```cmd
cd -
```

## man

Online manual (man page)

```cmd
man ls
```

![alt tag](https://i.imgur.com/3DDi208.png)

You can also use

```cmd
ls --help
```

![alt tag](https://i.imgur.com/ZSZjuVC.png)

## pwd

View the current path

```cmd
pwd
```

## ls

* [Youtube Tutorial - Linux Command Tutorial - ls](https://youtu.be/3Zy1AWuDUHE)

List files

```cmd
ls -l
```

`-l` displays detailed information (file permissions).

It is also equivalent to directly typing (lowercase L)

```cmd
ll
```

In Linux, files have four types of permissions:

*   Readable (r), represented by the number 4.
*   Writable (w), represented by the number 2.
*   Executable (x), represented by the number 1.
*   No permission (-), represented by the number 0.

For clarity, I've organized it into a table :yum:

| Character | Permission Score |
|:---------:|:----------------:|
| r (read)  | 4                |
| w (write) | 2                |
| x (execute)| 1                |
| - (no permission) | 0        |

As shown in the figure below,

![alt tag](https://i.imgur.com/AzfYBhf.png)

Next, let's explain the meaning of each column.

![alt tag](https://i.imgur.com/3TMcAtC.png)

*   First column (labeled 1 in the figure), user permissions.

    Composed of 10 characters.

    The first character represents the file type (`-` for a file, `d` for a directory, `l` for a link file).

    The second, third, and fourth characters represent the access permissions of the file owner.

    The fifth, sixth, and seventh characters represent the access permissions of the members of the file owner's group.

    The eighth, ninth, and tenth characters represent the access permissions of other users.

    Let's look at an example, `drwxr-xr-x`.

    This means it is a directory.

    The owner has read, write, and execute permissions.

    The group only has read and execute permissions.

    Other users only have read and execute permissions.

    For clarity, I've organized it into a table :yum:

| | Owner | Group | Other Users |
|---|:---:|:---:|:---:|
| d | rwx | r-x | r-x |
| Represents a directory | Has read, write, execute permissions | Only has read, execute permissions | Only has read, execute permissions |

    Its permission score is 755

| Identity | Permissions | Score |
|:---:|:---:|:---:|
| owner | rwx | 4+2+1 =7 |
| group | r-x | 4+0+1 =5 |
| others | r-x | 4+0+1 =5 |

*   Second column (labeled 2 in the figure), number of files.
*   Third column (labeled 3 in the figure), owner.
*   Fourth column (labeled 4 in the figure), group.
*   Fifth column (labeled 5 in the figure), file size.
*   Sixth column (labeled 6 in the figure), file creation time.
*   Seventh column (labeled 7 in the figure), file name.

Sort `ls` by time

```cmd
ls -t
```

List specific files (list files with the `.py` extension)

```cmd
ls *.py
```

`-h` parameter, displays file or directory size in KB, MB, GB units.

```cmd
ls -l -h
```

Display all files (including hidden files)

```cmd
ls -a
```

You can also use

```cmd
ls -al
```

List the contents of a directory directly

```cmd
ls Downloads
```

For example, in the home directory, list the contents of `Downloads` directly.

![alt tag](https://i.imgur.com/Dal7aSn.png)

sort

```cmd
ls -S
```

Write the output result stdout to a file, you can use redirect `>` (will not be displayed on the screen)

```cmd
ls -lS > file.txt
```

Count the number of files in a path

```cmd
ls | wc -l
```

## sort

* [Youtube Tutorial - Linux Command Tutorial - sort, uniq](https://youtu.be/5G9gRLPBW_U)

As the name implies, it's for sorting.

Suppose there is a `test.txt` as follows,

```txt
c 2
a 4
y 33
b 111
e 44
j 3
k 12
```

By default, it sorts based on the beginning of the line.

```cmd
❯ sort test.txt
a 4
b 111
c 2
e 44
j 3
k 12
y 33
```

To reverse the order, you can add `-r`, `--reverse` reverse the result of comparisons

```cmd
❯ sort -r test.txt
y 33
k 12
j 3
e 44
c 2
b 111
a 4
```

It can also be used with other commands, such as

```cmd
cat test.txt | sort
```

Sort by a specified field.

```cmd
❯ sort -n -k 2 test.txt
c 2
j 3
a 4
k 12
y 33
e 44
b 111
```

`-n`, `--numeric-sort` means sort numerically.

`-k`, `--key=KEYDEF` means sort by a specified field. Here, the second field is specified.

A quick supplement, if it's separated by spaces as above, no special settings are needed (because it's the default).

If your file is separated by commas as follows,

you need to add `-t` to set your delimiter.

`-t`, `--field-separator=SEP` use SEP instead of non-blank to blank transition.

`test2.txt` is as follows,

```txt
c,2
a,4
y,33
b,111
e,44
j,3
k,12
```

```cmd
❯ sort -n -t , -k 2 test2.txt
c,2
j,3
a,4
k,12
y,33
e,44
b,111
```

Use `-t` to set `,` as the delimiter.

## uniq

* [Youtube Tutorial - Linux Command Tutorial - sort, uniq](https://youtu.be/5G9gRLPBW_U)

Used to find (remove) duplicate lines.

```cmd
❯ uniq --help
......
Filter adjacent matching lines from INPUT (or standard input),
writing to OUTPUT (or standard output).
......
Note: 'uniq' does not detect repeated lines unless they are adjacent.
You may want to sort the input first, or use 'sort -u' without 'uniq'.
......
```

Please note, when using `uniq`, please execute `sort` first.

Because `uniq` only compares adjacent lines, you must `sort` before using `uniq`.
(The help text also says `uniq` does not detect repeated lines unless they are adjacent)

Example `test.txt` is as follows,

```txt
11
33
66
44
55
66
55
11
66
33
```

If you don't execute `sort` first and directly execute `uniq`, you will find it is ineffective.

```cmd
❯ uniq test.txt
11
33
66
44
55
66
55
11
66
33
```

Remove duplicate lines from the file,

```cmd
❯ sort test.txt | uniq
11
33
44
55
66
```

You can also use `sort -u` instead,

```cmd
❯ sort -u test.txt
11
33
44
55
66
```

`-u`, `--unique` with -c, check for strict ordering.

Count the number of occurrences of duplicate lines,

```cmd
❯ sort test.txt | uniq -c
      2 11
      2 33
      1 44
      2 55
      3 66
```

`-c`, `--count` prefix lines by the number of occurrences.

If you have blank lines, you can add the `sed` command to remove them (as in the example below)

```cmd
sort test.txt | sed '/^$/d' | uniq -c
```

Output all duplicate lines,

```cmd
❯ sort test.txt | uniq -D
11
11
33
33
55
55
66
66
66
```

`-D` print all duplicate lines

Output only duplicate lines (display only once),

```cmd
❯ sort test.txt | uniq -d
11
33
55
66
```

`-d`, `--repeated` only print duplicate lines, one for each group

Output only non-duplicate lines,

```cmd
❯ sort test.txt | uniq -u
44
```

`-u`, `--unique` only print unique lines

## cut

Used to extract parts of characters.

Example `test.txt`

```text
123
234
567
890
```

Extract the 2nd to 3rd characters

```cmd
❯ cut -c 2-3 test.txt
23
34
67
90
```

`-c`, `--characters=LIST` select only these characters

Extract from the 2nd character to the end

```cmd
❯ cut -c 2- test.txt
23
34
67
90
```

Extract the 1st and 3rd characters

```cmd
❯ cut -c 1,3 test.txt
13
24
57
80
```

Exclude the 2nd character

```cmd
❯ cut -c 2 test.txt --complement
13
24
57
80
```

`--complement` complement the set of selected bytes, characters or fields.
(Complements other characters, meaning it excludes the specified characters)

## tee

Simultaneously write the output result stdout to a file and display it on the screen (overwrites file.txt)

```cmd
ls | tee file.txt
```

Simultaneously write the output result stdout to a file and display it on the screen (appends to file.txt)

```cmd
ls | tee -a file.txt
```

## touch

Often used to create empty files

```cmd
touch file.py
```

You can also create multiple empty files at once this way (`file1.py` to `file10.py`)

```cmd
touch file{1..10}.py
```

## su

Switch to a different user

```cmd
su <username>
```

## sudo

Add a new user

```cmd
sudo useradd <username>
```

Set a user's password

```cmd
sudo passwd <username>
```

Delete a user

```cmd
sudo userdel <username>
```

Add a new group

```cmd
sudo groupadd <groupname>
```

Delete a group

```cmd
sudo groupdel <groupname>
```

Add a user to a group

```cmd
sudo usermod -g <groupname> <username>
```

View all users

```cmd
sudo cat /etc/passwd
```

View all groups

```cmd
sudo cat /etc/group
```

I don't know if everyone has this problem, but it's very troublesome to have to type your password every time :expressionless:

Here is a method for everyone, but be careful, it's the `-S` command.

```text
The -S (stdin) option causes sudo to read the password from
the standard input instead of the terminal device.
```

Simply put, you type your password first, so you don't have to type it again. Here's an example:

```cmd
echo YourPwd | sudo -S groupadd <groupname>
```

## chmod

* [Youtube Tutorial - Linux Tutorial - chmod](https://youtu.be/qwk4Pzgtf2I)

`chmod` is short for change mode.

Change file permissions

```cmd
chmod XXX filename
```

For example, to set permissions to `rw-rw-r--`,

| Identity | Permissions | Score |
|:---:|:---:|:---:|
| owner | rw- | 4+2+0 =6 |
| group | rw- | 4+2+0 =6 |
| others | r-- | 4+0+0 =4 |

```cmd
chmod 664 README.md
```

Commonly used permission modification commands

```cmd
# Only the owner has read and write permissions
sudo chmod 600 ×××
```

```cmd
# The owner has read and write permissions, group and others only have read permission
sudo chmod 644 ×××
```

```cmd
# The owner has read, write, and execute permissions
sudo chmod 700 ×××
```

```cmd
# The owner, group, and others all have read and write permissions
sudo chmod 666 ×××
```

```cmd
# The owner, group, and others all have read, write, and execute permissions, basically full access
sudo chmod -R 777 xxx
```

`-r` `-R` stands for recursive (all files and subdirectories under the directory will be changed).

There is another way to change permissions using symbols.

Before introducing it, take a look at the table below :wink:

| | u = user | | | |
|---|---|---|---|---|
| | g = group | + (add) | r = read | |
| chmod | | - (remove) | w = write | file or directory |
| | o = other | = (set) | x = execute | |
| | a = all | | | |

For example, to set the permissions of `hello` to `rw-rw-r--`,

| Owner(u) | Group(g) | Other Users(o) |
|:---:|:---:|:---:|
| rw- | rw- | r-- |

```cmd
chmod ug=rw,o=r hello
```

![alt tag](https://i.imgur.com/QgNuNel.png)

Another example, to set the permissions of `hello` to `rwxr-xr--`,

```cmd
chmod u=rwx,g=rx,o=r hello
```

![alt tag](https://i.imgur.com/WlX8wPL.png)

Now, suppose I want to add execute permission (x) to everyone (all users and groups)

```cmd
chmod a+x hello
```

![alt tag](https://i.imgur.com/KLiwPXX.png)

Remove execute permission (x) from everyone

```cmd
chmod a-x hello
```

You will find that everyone's execute permission (x) has disappeared.

![alt tag](https://i.imgur.com/O8gh3Is.png)

After this series of exercises, I believe everyone understands.

If you don't understand, watch it a few more times :satisfied:

## chown

Modify the owner and group of a file or directory.

Modify the owner of a file or directory

```cmd
# Change the owner of README.md (file) to twtrubiks (user)
chown twtrubiks README.md
```

Modify the group of a file or directory

```cmd
# Change the group of README.md (file) to twtrubiksgroup (group)
chown :twtrubiksgroup README.md
```

Simultaneously modify the owner and group of a file or directory

```cmd
# Change the owner of README.md (file) to twtrubiks (user) and
# the group of README.md (file) to twtrubiksgroup (group)
chown twtrubiks:twtrubiksgroup README.md
```

## ln

* [Youtube Tutorial - Linux Command Tutorial - ln (Symbolic Link)](https://youtu.be/jdZsO2GAf2I)

There are two types, hard link and symbolic link (soft link).

First, let's introduce hard links. Note, hard links are not allowed for directories.

```cmd
ln /home/twtrubiks/Downloads/odoo-git/README.md
```

![alt tag](https://i.imgur.com/ioJXBRw.png)

The characteristic of a hard link is that no matter which file is deleted, the file will be preserved. Unless you delete the last file as well.

In other words, a hard link of a file and the original file have no substantial difference.

Hard links are not allowed for directories, only for files.

A symbolic link, also called a soft link, is basically similar to a shortcut in Windows :smile:

```cmd
ln -s /home/twtrubiks/Downloads/odoo-git/ dir-link
```

![alt tag](https://i.imgur.com/JGhlQZd.png)

When the original file is deleted, its symbolic link can no longer read the file.

A symbolic link of a file and the original file are two different things.

Symbolic links are allowed for files and directories.

## zip unzip

zip 3.0 will preserve file permissions and ownership.

```cmd
sudo apt-get install zip unzip
```

zip

```cmd
zip -r <compressed_filename> <file_to_compress>
zip -r file.zip file
```

unzip

```cmd
unzip <file_to_unzip> -d <target_directory>
unzip file.zip -d zip_extract
```

If you want to unzip directly to the current directory, you can use `.`

```cmd
unzip file.zip -d .
```

## tar

tar **will** preserve file permissions and ownership.

Compress to `.tar` format

```cmd
tar cvf filename.tar source-folder
```

Decompress `.tar` format

```cmd
tar xvf filename.tar
```

## unrar

```cmd
sudo apt-get install unrar
```

Unzip `filename.rar` to the directory

```cmd
unrar e filename.rar
```

List the contents of `filename.rar`

```cmd
unrar l filename.rar
```

Test if `filename.rar` is complete and correct

```cmd
unrar t filename.rar
```

## wget

Download tool

```cmd
sudo apt-get install wget
```

Download URL command

```cmd
wget http://ftp.gnu.org/gnu/wget/wget-1.20.3.tar.gz
```

To specify a filename, add `-O`

```cmd
wget -O wget.tar.gz http://ftp.gnu.org/gnu/wget/wget-1.20.3.tar.gz
```

## scp

Full name is Securely Copy.

This method is suitable for transferring files between Linux and Linux, and also between Linux and Windows.

Suppose the Linux IP is 192.168.56.101. The command to check the IP is as follows:

```cmd
ip addr show
```

![alt tag](https://i.imgur.com/AlAeRoD.png)

Make sure `openssh-server` is installed

```cmd
sudo apt-get install openssh-server
```

Use `ssh localhost` to test

![alt tag](https://i.imgur.com/nYo5NNn.png)

After everything is normal.

To send a file from Windows to Linux (IP is 192.168.56.101),

execute the following command in the Windows cmd:

```cmd
scp -rp file linux_user@ip:target_path
```

`-r` stands for recursive.

`-p` stands for preserving the original file's content (Preserves modification).

```cmd
scp -rp file twtrubiks@192.168.56.101:/home/twtrubiks
```

![alt tag](https://i.imgur.com/0nBrt00.png)

Next, to get a file from Linux back to Windows

```cmd
scp -P 22 linux_user@ip:target_path destination_path
```

`-P` stands for explicitly specifying the connection port (remote host).

```cmd
scp -P 22 twtrubiks@192.168.56.101:/home/twtrubiks/linux_file.md .
```

`.` represents the current path (you can also specify other paths).

![alt tag](https://i.imgur.com/aMnNlGI.png)

The same principle applies to transferring files between Linux machines :smile:

## mv

* [Youtube Tutorial - Linux Command Tutorial - mv](https://youtu.be/VhyzaEaGnL8)

move (rename) files, **move files** or **rename files**.

Modify the name of a directory or file

```cmd
mv folder folder-new
mv README.md README_MV.md
```

Move a file

```cmd
mv README.md /examples
```

```cmd
mv file.md example/
```

Other parameter descriptions (parameters can be used together),

Interactive mode, CLI will ask you if you want to overwrite files

```cmd
mv -i source_file path_to_destination/
```

Only update files that are different between the source and destination

```cmd
mv -u source_file path_to_destination/
```

## rm

* [Youtube Tutorial - Linux Command Tutorial - rm](https://youtu.be/JqKjBZMXn_I)

Delete a file

```cmd
rm file.md
```

Delete a directory

```cmd
rm -rf mydir
```

`-r` stands for recursive deletion. (Will delete all files in the directory)

`-f` stands for force deletion (will not show a warning).

Or use the `rmdir` command,

```cmd
rmdir mydir_name
```

Note that the directory to be removed must be empty, otherwise it cannot be removed.

Delete files with a specific extension,

```cmd
rm -f *.zip
```

You can also do this

```cmd
rm -f *demo.zip
```

## cp

* [Youtube Tutorial - Linux Command Tutorial - cp](https://youtu.be/ORl0YUGY728)

Copy a directory

```cmd
cp -r path_to_source/ path_to_destination/
```

`-r` `-R` stands for recursive.

If `path_to_destination` does not exist, it will be created automatically;

If it exists, it will be used directly.

To copy only the contents of a directory,

```cmd
cp -r dir_1/. dir_2
cp -r dir_1/. .
```

`.` represents the contents of the directory, and can also represent the current location.

Sometimes you want to preserve the permissions when copying, so you add `-p`.

```cmd
cp -r --preserve=all path_to_source/ path_to_destination/
```

`-p` `--preserve` means to copy the permissions and owner as well.

Other parameter descriptions (parameters can be used together),

Interactive mode, CLI will ask you if you want to overwrite files

```cmd
cp -i source_file path_to_destination/
```

Do not ask, overwrite files directly

```cmd
cp -n source_file path_to_destination/
```

Only update files that are different between the source and destination

```cmd
cp -u source_file path_to_destination/
```

Print information

```cmd
cp -v source_file path_to_destination/
```

## find

Find files

Find a file or directory

```cmd
sudo find / -name "dir-name"
sudo find / -name "file-name"
sudo find / -name "*.conf"
```

Find a file named `README.md` in the current directory

```cmd
find . -name README.md
```

## source

The `source` command is usually used for newly modified initialization files to make them take effect immediately without rebooting (or logging out and logging back in).

Here's an example,

```cmd
source demo.sh
```

This reads and executes `demo.sh` in the current shell, and `demo.sh` **needs** to have execute permission.
(Execute permission means `chmod +x demo.sh`)

The `source` command can also be abbreviated as `.`

```cmd
. demo.sh
```

## sh or bash

When executing with `sh` or `bash`, execute permission is **not required**.
(Execute permission means `chmod +x demo.sh`)

```cmd
sh demo.sh
bash demo.sh
```

Custom value passing, below is `test.sh`

```sh
#!/bin/bash
echo "$0" # filename
echo "$1"
echo "$2"
echo "$3"
```

Execution result

```cmd
sh test.sh a1 a2 a3
```

## ./

When executing directly with `./`, execute permission is **required**.
(Execute permission means `chmod +x demo.sh`)

When you execute

```cmd
./demo.sh

chmod +x demo.sh
./demo.sh
```

You will see a message like `bash: ./demo.sh: Permission denied`.

The fix is as follows,

```cmd
chmod +x demo.sh
./demo.sh
```

## where

Find a path.

For example, to find the path of `python3`

```cmd
where python3
which python3
whereis python3
```

## tail

Display the last few lines of a file (default is the last 10 lines)

```cmd
tail README.md
```

Display multiple files at once

```cmd
tail README_1.md README_2.md
```

Specify the last N lines to display

```cmd
tail -n 5 README.md
```

```cmd
tail README.md -n 5
```

Continuously display updated content, usually used for servers or viewing logs

```cmd
tail -f README.md
```

Can also be used with `grep`. The following command continuously tracks a certain value.

```cmd
tail -f README.md | grep -n 'test'
```

If the `test` content above is outside the last 10 lines,

you won't find it. You either need to increase the number of lines or add new values.

Open another terminal and execute the following command,

to add "test" to the end of the file.

```cmd
echo "test" >> README.md
```

Then you will see the message.

## head

Since there is `tail`, there must be `head` :smile:

```cmd
head text.py
```

Displays the first 10 lines by default.

You can use the `-n` command to specify the first `n` lines to display

```cmd
head -n 3 text.py
```

## file

Check the file type

```cmd
file README.md
```

## cat

Display the contents of a file on the terminal

```cmd
cat README.md
```

Display line numbers

```cmd
cat -n README.md
```

`cat` can also write to a file

```cmd
cat <<EOT >> hello_4.txt
line 1
line 2
line 3
EOT
```

Use with `grep` to filter for "test"

```cmd
cat README.md | grep 'test'
```

You can also be more aggressive and find all files ending in `.log` in the directory

```cmd
cat *.log | grep 'test'
```

## clear

clear the terminal screen, shortcut is Ctrl+L

```cmd
clear
```

## grep

```cmd
# format
grep match_pattern file_name
```

Add `--color` to color the keywords for clearer display.

```cmd
grep --color "search name" README.md
```

Add `-C` to display a number of lines before and after the match.

```cmd
grep --color -C 2 "search name" README.md
```

You can also search multiple files at once

```cmd
grep "name" README_1.md README_2.md
```

You can also use the wildcard `*`

```cmd
grep "print" *.py
```

Exclude a certain character

```cmd
grep -v "match_pattern" README.md
```

`-v`, `--invert-match` select non-matching lines

If you want to exclude certain characters and search for others,

you can use it as follows according to your needs,

```cmd
grep -v "ignore" README.md | grep --color "match_pattern"
```

Search the contents of the current directory

```cmd
grep -r "search name" .
```

case insensitive case

```cmd
grep -i "name" README_1.md
```

Display line numbers

```cmd
grep -n "name" README_1.md
```

Only lines that exactly match `:80` will be retrieved

```cmd
grep -w ':80' README_1.md
```

`-w`, `--word-regexp` only compare whole words.

## sed

This command can be used to quickly search, replace, and delete text.

`sed` mainly processes **lines**, and it processes a copy of the file, not the original.

Syntax

```cmd
sed -i '/match_string/d' textfile
```

`-i` must be added to write to your `textfile`, otherwise it will only be displayed on the terminal.

Delete empty lines

```cmd
sed -i '/^$/d' textfile
```

Delete lines containing the number 7

```cmd
sed -i '/7/d' textfile
```

Delete lines 1 to 5

```cmd
sed -i '1,5d' textfile
```

Delete all lines between `hello1` and `hello2`

```cmd
sed -i '/hello1/, /hello2/d' textfile
```

Replacement syntax

```cmd
sed -i 's/match_string/replacement_string/' textfile
```

Replace the first occurrence of `a` with `A` on each line

```cmd
sed -i 's/a/A/' textfile
```

Replace all occurrences of `a` with `A` on each line

```cmd
sed -i 's/a/A/g' file
```

`g` means replace all matching strings

Print only lines containing `test`

```cmd
sed -n '/test/p' test.txt
```

`-n`, `--quiet`, `--silent` suppress automatic printing of pattern space.

`p` Print the current pattern space.

`sed` can also print specified lines of a file.

```cmd
❯ cat test.txt
1
2
3
4
5
6
```

Display a specific line, display line 5

```cmd
❯ sed -n 5p test.txt
5
```

Display lines 3 and 5

```cmd
❯ sed -n -e 3p -e 5p test.txt
3
5
```

`-e` script, `--expression=script`

add the script to the commands to be executed

Display lines 3 to 5

```cmd
❯ sed -n 3,5p test.txt
3
4
5
```

Display lines 1 to 3, and line 5

```cmd
❯ sed -n -e 1,3p -e 5p test.txt
1
2
3
5
```

## awk

This command is a very powerful text analysis tool.

Suppose our output is as follows

![alt tag](https://i.imgur.com/GhPq6sZ.png)

Output the 2nd, 3rd, 5th, and 9th columns

```cmd
ll | awk '{print $2,$3,$5,$9}'
```

![alt tag](https://i.imgur.com/o1exYCq.png)

If you think it's ugly, you can use `printf` to format it

```cmd
ll | awk '{printf "%-5s %-5s %-5s %-5s\n", $2,$3,$5,$9}'
```

![alt tag](https://i.imgur.com/9RQj28o.png)

Now let's try to filter the data.

Extract the data where the permission score (2nd column) is 2 and the 3rd column is `twtrubiks`

```cmd
ll | awk '$2 == "2" && $3 == "twtrubiks" {print $0}'
```

`$0` represents the entire content of the line.

![alt tag](https://i.imgur.com/Il9jGFp.png)

You can also perform statistics.

Sum the permission scores (2nd column) (excluding "total")

First, exclude the data where the first column is "total".

`my_sum` is a variable we define.

```cmd
ll | awk '$1 != "total" {my_sum+=$2} END{print my_sum}'
```

![alt tag](https://i.imgur.com/o3yXZnT.png)

You can also write `if` logic.

Filter out the data where the permission score (2nd column) is 3.

Then print the current line number and convert the file name in the 9th column to uppercase.

```cmd
ll | awk '{if ($2 == "3") print NR, toupper($9)}'
```

`NR` current record number in the total input stream.

![alt tag](https://i.imgur.com/dzlbMAA.png)

`NF` number of fields in the current record.

Example `test.txt`

```cmd
❯ cat test.txt
-rw-rw-r-- 1 twtrubiks twtrubiks 5  4月  2 20:08 a.py
```

Current number of fields,

```cmd
❯ cat test.txt | awk '{print NF}'
9
```

Last field,

```cmd
❯ cat test.txt | awk '{print $NF}'
a.py
```

Display the first field,

```cmd
❯ cat test.txt | awk '{print $1F}'
-rw-rw-r--
```

## mkdir

Create a directory

```cmd
mkdir -p dir1/dir2
```

`-p` `--parents` means to automatically create parent directories. No error occurs if the directory already exists.

## kill

Force stop a program.

You need to find the program's PID first. The usage is as follows,

```cmd
kill -9 PID
```

`-9` immediately force stops the program.

## killall

One difference between `killall` and `kill` is that you can use the program name.

You don't need to find the program's PID first.

For example, to force stop `vlc`

```cmd
killall vlc
```

## history

History of entered commands

```cmd
history
```

```cmd
history | less
```

![alt tag](https://i.imgur.com/0YKqS3Y.png)

If I don't want to type a command today, I can directly type `!` + number, and it will automatically execute that command.

```cmd
!1848
```

Display the last entered command again (it is recommended to add `sudo`)

```cmd
!!
```

Can also be used with `grep`.

If I want to find the commands I have entered that contain `git`, I can use the following command

```cmd
history | grep git
```

If I don't want to display everything at once, I can use `less`

```cmd
history | grep git | less
```

## echo

Print the value of a shell variable in the shell.

Set `EDITOR`

```cmd
export EDITOR=vim
```

View the current `EDITOR`,

```cmd
echo $EDITOR
```

View the current shell,

```cmd
echo $SHELL
```

You can also set a default in the environment variables,

```cmd
❯ echo ${SECRET_KEY:-secrets}
secrets
```

When you have `SECRET_KEY` in your environment variables, it will be used. If it is not set,

the `secrets` you defined will be used.

`echo` can also write to a file.

Method one

```cmd
echo "line 1" >> hello_1.txt
```

Method two (write multiple lines)

```cmd
echo "line 1
line 2" >> hello_2.txt
```

Method three (write multiple lines)

```cmd
{
    echo 'line1;'
    echo 'line2;'
} >> hello_3.txt
```

## cal

Display the calendar

```cmd
cal
```

Display the previous, current, and next month

```cmd
cal -3
```

Display a specific month and year

Format

```cmd
cal month year
```

Example

```cmd
cal 12 2022
```

## du

* [Youtube Tutorial - Linux Command Tutorial - du(Disk Usage)](https://youtu.be/JZZoJnasnHE)

The `du` command is short for Disk Usage.

Before introducing `du`, let's look at an example.

Observe the `debian` directory using `ls -l -h`

![alt tag](https://i.imgur.com/lXgxQop.png)

But if you go into the directory, you will find that it is actually 17GB.

But why does it only show 4KB when viewed from the parent directory? :question:

![alt tag](https://i.imgur.com/eOTKWJj.png)

The reason is that `ls -l -h` does not display the actual size of the directory, only the so-called meta information.

So, if you want to see the actual size, a better way is to use the `du` command that we are about to introduce :smile:

View the `du` command help

```cmd
du --help
```

![alt tag](https://i.imgur.com/IQLpqnC.png)

```cmd
-s, --summarize       display only a total for each argument
                      (Equivalent to -d 0)

-h, --human-readable  print sizes in human readable format (e.g., 1K 234M 2G)
    --inodes          list inode usage information instead of block usage

-c, --total           produce a grand total

-d, --max-depth=N     print the total for a directory (or file, with --all)
                      only if it is N or fewer levels below the command
                      line argument;  --max-depth=0 is the same as --summarize


-a, --all             write counts for all files, not just directories
    --apparent-size   print apparent sizes, rather than disk usage; although
                      the apparent size is usually smaller, it may be
                      larger due to holes in ('sparse') files, internal
                      fragmentation, indirect blocks, and the like
```

The following two commands have the same function

```cmd
du -sh *
du --summarize --human-readable *
```

Using the previous example, you can see the actual size of the directory from the parent directory.

![alt tag](https://i.imgur.com/hHjxXDx.png)

You can also use it with `-d`, the depth of the directory. You will understand from the example below.

```cmd
du -d 2 -h
```

![alt tag](https://i.imgur.com/NdbqvSz.png)

## truncate

* [Youtube Tutorial - Linux Command Tutorial - truncate](https://youtu.be/w2pwD1AOhPI)

Shrink or extend the size of each FILE to the specified size.

The `truncate` command can shrink or increase the size of a file.

Before introducing this command, let's look at a suitable scenario :smile:

Sometimes we may want to reduce the size of a file to 0, that is, delete all the contents of the file,

but keep the file. This is a very suitable situation to use this command :smirk:

You might ask me, why not just delete the file and create a new one with the same name? :question:

The reason is simple. In the Linux world, files have permissions, so you have to pay attention to whether the newly created

file has the same permissions as before (otherwise it may cause errors). So a simpler

method is to use the `truncate` command. It will only clear the content (file size becomes 0),

and everything else remains the same.

View the `truncate` command help,

```cmd
truncate --help
```

![alt tag](https://i.imgur.com/rOXR79N.png)

```cmd
Mandatory arguments to long options are mandatory for short options too.
-c, --no-create        do not create any files
-o, --io-blocks        treat SIZE as number of IO blocks instead of bytes
-r, --reference=RFILE  base size on RFILE
-s, --size=SIZE        set or adjust the file size by SIZE bytes
    --help     display this help and exit
    --version  output version information and exit

SIZE may also be prefixed by one of the following modifying characters:
'+' extend by, '-' reduce by, '<' at most, '>' at least,
'/' round down to multiple of, '%' round up to multiple of.
```

Let's use the following example to illustrate.

Suppose there is a file `demo.txt` (as shown below)

![alt tag](https://i.imgur.com/nWoxmhn.png)

Use `truncate` to increase the size of `demo.txt` to 1M.

```cmd
truncate -s 1M demo.txt
```

![alt tag](https://i.imgur.com/jeZ6Rkp.png)

Note that `du -ah` displays apparent sizes (not disk usage), so the size will not change.

If you open `demo.txt`, you will find that it is filled with a lot of stuff, because the size has to become 1M :smile:

![alt tag](https://i.imgur.com/mgQNkNn.png)

Conversely, now use `truncate` to shrink `demo.txt` to 0.

![alt tag](https://i.imgur.com/9czKNL5.png)

If you open `demo.txt`, you will find that all the data inside has disappeared.

![alt tag](https://i.imgur.com/vmLwz96.png)

The `truncate` command is very suitable for clearing logs, setting the log size to 0, and keeping everything else the same.

Clear all log files

```cmd
sudo truncate -s 0 /var/log/**/*.log
```

## shred

* [Youtube Tutorial - Linux Command Tutorial - tldr, shred, sleep](https://youtu.be/RqI-DF1I8R0?t=172)

Destroy important files to prevent recovery software from restoring them.

Overwrite files to securely delete data.

The usage is very simple.

Shred `demo.txt`

```cmd
shred demo.txt
```

Shred `demo.txt` and leave zeroes

```cmd
shred --zero demo.txt
```

Overwrite the file 25 times (default is 3 times)

```cmd
shred -n25 demo.txt
```

Shred and delete it

```cmd
shred --remove demo.txt
```

## sleep

* [Youtube Tutorial - Linux Command Tutorial - tldr, shred, sleep](https://youtu.be/RqI-DF1I8R0?t=358)

You can delay for a specific time before executing the corresponding command.

sleep for 0.5 seconds

```cmd
sleep 0.5
```

sleep for 1 minute

```cmd
sleep 1m
```

sleep for 1 hour

```cmd
sleep 1h
```

Use with other commands, sleep for a second, then print "hello"

```cmd
sleep 1 && echo "hello"
```

## ps

`ps` is short for Process Status.

List the processes currently running in the shell

```cmd
ps
```

List all processes

```cmd
ps -A
```

`-A`, `-e` all processes

List all processes using BSD format

```cmd
ps aux
```

`a` all with tty, including other users
`x` processes without controlling ttys
`u` user-oriented format

Use with `grep` to find the corresponding PS

```cmd
ps aux | grep "postgres -c"
```

Find the corresponding PS using PID

```cmd
ps -o pid,rss,vsz,sz,comm -p PID
```

The `RSS` value is the same as the `RES` in the `top` command.

Both can be considered as the actual physical memory occupied.

`-o`, `o`, `--format <format>` user-defined format.

`-p`, `p`, `--pid <PID>` process id

## tree

```cmd
sudo apt install tree
```

```cmd
❯ tree
.
├── Git-Flow
│   └── README.md
├── git_submodule_turorial.md
├── git_subtree_turorial.md
```

Display only one level of files

`-L` level means Descend only level directories deep.

```cmd
❯ tree -L 1
.
├── auto_crawler_ptt_beauty_image
├── BBBB
├── django-celery-tutorial
├── django-docker-redis-tutorial
```

## nslookup

Find the IP and various information of a URL

```cmd
❯ nslookup google.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	google.com
Address: 142.251.43.14
Name:	google.com
Address: 2404:6800:4012:3::200e
```

## NTP

Full name is The Network Time Protocol.

The main function is time synchronization.

If you find that the server time is inaccurate, you need to install this to ensure that the time is automatically synchronized.

```cmd
sudo apt install ntp
cat /etc/ntp.conf
sudo systemctl status ntp
```

Basically, the installation will take effect automatically.

## SSH

Usually, our ssh format is `ssh user@xx.xx.xx.xx`.

But this is too troublesome, and sometimes you forget.

Using `ssh_config` is faster and more convenient.

First, configure it

```cmd
sudo vim ~/.ssh/config
```

The content is roughly as follows

```cmd
# example
Host my-remote
HostName xx.xxx.xx.xx
Port 22
IdentityFile ~/.ssh/id_rsa
IdentitiesOnly yes
User root
```

Then you only need to type `ssh my-remote` in the terminal.

### SSH Prevent Disconnection

I don't know if everyone has experienced this when ssh-ing to a remote machine.

After a while of inactivity, it disconnects by itself. If you want to improve this,

go to `sudo vim ~/.ssh/config` and add the following,

```cmd
# Prevent disconnection
Host *
    ServerAliveInterval 100
```

This means sending a packet to the host every 100 seconds to maintain the connection.

The unit is seconds, so you won't be disconnected after a short period of inactivity.

## Log in to a remote Linux without a password

### Method One

First, make sure there is a `.ssh` directory on the Linux machine. If not,

use the following command to create it (and set permissions),

```cmd
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

It's troublesome to enter a password every time you log in to a remote Linux machine. Is there a way to avoid entering a password? :question:

Yes, first use `ssh-keygen` on your local machine to generate a key pair.

```cmd
ssh-keygen
```

Then you will have two keys,

`id_rsa.pub`: public key: to be placed on the remote Linux server.

`id_rsa`: private key: protect it yourself, it is equivalent to your Linux password.

First, transfer your local `id_rsa.pub` to the remote Linux server.

Then execute the following command on the Linux machine

(Put the public key in `~/.ssh/authorized_keys`)

```cmd
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

Let's test it :smile:

`ssh twtrubiks@192.168.56.101`

![alt tag](https://i.imgur.com/97ndrP8.png)

You can log in without entering a password :thumbsup:

### Method Two

You can also use `ssh-copy-id` to do this.

```cmd
ssh-copy-id -i ~/.ssh/id_rsa.pub twtrubiks@192.168.56.101
```

![alt tag](https://i.imgur.com/eR5TIJ3.png)

In fact, whether it's method one or method two, it's just adding the key to `authorized_keys`

in `/home/<user>/.ssh` :smile:

![alt tag](https://i.imgur.com/j4BRI1J.png)

## Log in to a remote Linux as the root user

Note, this is usually not done :exclamation:

Although this method can be more dangerous, I will explain it anyway :joy:

First, set the root password by executing the following command

```cmd
sudo passwd root
```

![alt tag](https://i.imgur.com/dsSrBJX.png)

Then use `su -` to test the root password

![alt tag](https://i.imgur.com/nmU9cgk.png)

After the test is complete, execute `ssh root@192.168.56.101`

![alt tag](https://i.imgur.com/BF4JWnO.png)

You will find that you keep getting `Permission denied, please try again.`

At this time, you need to modify the root's ssh settings on the Linux machine.

```cmd
sudo vim /etc/ssh/sshd_config
```

Find `PermitRootLogin without-password`

![alt tag](https://i.imgur.com/NO85xui.png)

Change it to `PermitRootLogin yes`

![alt tag](https://i.imgur.com/xpyfpwW.png)

Finally, remember to restart `sshd` to make it take effect (or reboot)

```cmd
systemctl restart sshd
```

Successfully logged in as root :satisfied:

![alt tag](https://i.imgur.com/Au4wt32.png)

## Properly secure the server

A more secure practice is usually to disable `PermitRootLogin` and `PasswordAuthentication`,

and only enable `PubkeyAuthentication`. But here you must be careful to put your key on the

server, otherwise if you accidentally log out after setting it, it will be very troublesome :expressionless:

(Because you can't log in with a password, and you forgot to put the key on the server)

Modify

```cmd
sudo vim /etc/ssh/sshd_config
```

Disable root login, set `PermitRootLogin` to `no`,

![alt tag](https://i.imgur.com/W6KBiXS.png)

Disable password login, set `PasswordAuthentication` to `no`,

![alt tag](https://i.imgur.com/L9WPRq5.png)

Allow `PubkeyAuthentication`, set to `yes`

![alt tag](https://i.imgur.com/iYyaAQ8.png)

Supplement, there is also a way to use PAM Authentication `UsePAM` (AWS uses this method)

![alt tag](https://i.imgur.com/g3MdnKC.png)

As the description says, if you want to use only PAM Authentication, you can also set `ChallengeResponseAuthentication` to `no`.

Finally, remember to restart `sshd` to make it take effect (or reboot)

```cmd
systemctl restart sshd
```

## Set `/etc/hosts`

```cmd
sudo vim /etc/hosts
```

The following content means that on your local machine, `twtrubiks.com` is equal to `127.0.0.1`.

You can adjust it according to your needs.

```cmd
127.0.0.1     localhost
127.0.1.1     twtrubiks.com
```

## Desktop environment wayland or x11

Enter the following command to check.

`x11` is older, but has better compatibility with software.
(Kubuntu 22.04 defaults to x11)

```cmd
❯ echo $XDG_SESSION_TYPE
x11
```

`wayland` is newer, but has lower compatibility with software.
(debian 12 KDE default desktop environment)

```cmd
❯ echo $XDG_SESSION_TYPE
wayland
```

If you are using `wayland`, please install `pipewire`

```cmd
sudo apt install pipewire

systemctl --user start pipewire
```

Otherwise, you will find that you cannot record video or share your screen.

## Bluetooth connection

If you cannot use Bluetooth (debian12) and keep failing to pair,

please install the following and reboot.

```cmd
sudo apt install pipewire-audio
```

## Other information

System information

```cmd
uname -a
```

View CPU

```cmd
cat /proc/cpuinfo
```

View total RAM size

```cmd
grep MemTotal /proc/meminfo
```

View available RAM

```cmd
grep MemFree /proc/meminfo
```

You can also use

```cmd
free -m
```

View disk space (disk free space, Human Readable)

```cmd
df -h
```

Information on all drives (List Drives and Mounts)

```cmd
lsblk
```

Display information for all devices, UUID

```cmd
blkid
```

![alt tag](https://i.imgur.com/8a6V7fq.png)

Current drive mount status (auto-mount on boot)

```cmd
cat /etc/fstab
```

![alt tag](https://i.imgur.com/79WxI5w.png)

View all pci

```cmd
lspci
```

View all usb

```cmd
lsusb
```

Can also be used with `grep` to search, only search for content containing `VirtualBox`

```cmd
lsusb | grep VirtualBox
```

View IP

```cmd
ip a
```

external ip Address, `ifconfig.me` is a website.

```cmd
curl ifconfig.me
```

View current computer information, CPU, RAM, etc.

```cmd
top
```

`htop` is recommended (more clear information), it is recommended to refer to [htop-tutorial](https://github.com/twtrubiks/linux-note/tree/master/htop-tutorial) - htop tutorial :thumbsup:

Modify screen brightness through `xrandr`.

💢 If you are using `wayland`, this tool will not work, you must use `x11`.
Temporarily can't find a replacement software 😞

First, check the screen name

```cmd
xrandr | grep " connected" | cut -f1 -d " "
```

Set brightness (0 - 1)

```cmd
xrandr --output DP-1 --brightness 0.7
```

View Linux startup time through `systemd-analyze`

```cmd
systemd-analyze
```

View details

```cmd
systemd-analyze blame
```

If you want to see how long this system has been installed on this computer,

you can use the following command, because this directory is rarely modified,

so you can refer to its modification time

```cmd
stat /lost+found/
```

### Shutdown and restart commands

You can use the following command to view the command description

```cmd
shutdown --help
```

![alt tag](https://i.imgur.com/lDXENf0.png)

Shutdown immediately

```cmd
shutdown -h now
```

Shutdown at a specified time

```cmd
shutdown -h 22:30
```

![alt tag](https://i.imgur.com/CE9p1gt.png)

Cancel shutdown (for example, if you want to cancel after specifying a time, as shown in the figure above)

```cmd
shutdown -c
```

Simulate shutdown (can be used to confirm if the settings are correct)

```cmd
shutdown -k 9:30
```

Reboot

```cmd
reboot
```

View shutdown history

```cmd
last -x shutdown
```

View reboot history

```cmd
last -x reboot
```

System boot time

```cmd
uptime -s
```

last system boot time

```cmd
who -b
```

### How to enter the tty interface

Sometimes when booting, you may get stuck on a black screen because the driver is not installed.

At this time, you can enter the tty interface to install the driver (what you need).

Enter tty shortcut `Ctrl+Alt+F2`

Exit tty shortcut `Ctrl+Alt+F7` or `Ctrl+Alt+(F2/F3/F4)`

### swap

If your local RAM is large enough, you can consider turning it off.
(Some distros have it enabled by default)

Turn off swap

```cmd
sudo swapoff -a
```

Turn on swap

```cmd
sudo swapon -a
```

The following command can help you find out which programs are using how much swap

```cmd
(echo "COMM PID SWAP"; for file in /proc/*/status ; do awk '/^Pid|VmSwap|Name/{printf $2 " " $3}END{ print ""}' $file; done | grep kB | grep -wv "0 kB" | sort -k 3 -n -r) | column -t
```

![alt tag](https://i.imgur.com/uibxLSu.png)

## install packages

install

```cmd
sudo apt-get install xxx
```

If you only have a `.deb` file, you can use the following method

```cmd
sudo apt install ./xxx.deb
```

Select an installable version (e.g., firefox)

```cmd
sudo apt-cache policy firefox
```

Install a specific version (e.g., firefox)

```cmd
sudo apt install firefox=VERSION
```

Lock the version (disable automatic updates),

You can use the following command (e.g., firefox)

```cmd
sudo apt-mark hold firefox
```

Although you will still see that there is an updated version when you run `sudo apt-get update`, it will skip the update.

Example is as follows, it will be skipped automatically.

```cmd
❯ sudo apt upgrade -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  firefox-esr
```

Query packages with disabled automatic updates

```cmd
❯ sudo apt-mark showhold
firefox-esr
```

If you want to restore it today, you can use the following command (e.g., firefox)

```cmd
sudo apt-mark unhold firefox-esr
```

update list (update the latest information and list of packages)

```cmd
sudo apt-get update
```

The software sources will be placed in the `/etc/apt/sources.list.d` path.

upgrade (update software to the latest version)

```cmd
sudo apt-get upgrade
```

Only update specific software, for example `vivaldi`,

First update and then install means updating the software.

```cmd
sudo apt update && sudo apt install vivaldi-stable
```

remove

```cmd
sudo apt-get --purge autoremove xxxx
```

Clean up unnecessary packages (`.deb`)

```cmd
sudo apt autoclean
```

Clear unnecessary dependencies

```cmd
sudo apt autoremove
```

## ppa add/remove

add

```cmd
sudo apt-add-repository ppa:xxxx
sudo apt update
```

remove

```cmd
sudo add-apt-repository -r ppa:xxxx
sudo apt update
```

## Cannot enter bios

```cmd
sudo vim /etc/default/grub
```

You should see a similar screen

```text
GRUB_DEFAULT="0"
GRUB_TIMEOUT_STYLE="hidden"
GRUB_TIMEOUT=10   <<<<<<
GRUB_DISTRIBUTOR="`lsb_release -i -s 2> /dev/null || echo Debian`"
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash usbcore.autosuspend=-1"
GRUB_CMDLINE_LINUX=""
```

Make the `GRUB_TIMEOUT` time longer, because it may be too fast, causing you to not have enough time to press the key 🤣

Also remember to execute the following command to update

```cmd
sudo update-grub
```

## Skip grub guide

```cmd
sudo vim /etc/default/grub
```

Set `GRUB_TIMEOUT` to 0 to skip the grub guide.

Also remember to execute the following command to update

```cmd
sudo update-grub
```

## remove snap

In ubuntu, snap is installed by default. I personally don't like it very much.

Because it is maintained by a private company :smile:

The following is the command to remove snap.

First use `snap list` to view all installed packages.

Then remove them one by one using `sudo snap remove --purge firefox`.

After all are removed, remove snap completely

```cmd
sudo apt remove --purge snapd
```

Also delete the related folder `rm -rf ~/snap`.

To prevent snap from being installed again when installing other packages, please execute the following command

```cmd
sudo cat <<EOF | sudo tee /etc/apt/preferences.d/nosnap.pref
Package: snapd
Pin: release a=*
Pin-Priority: -10
EOF
```

Add firefox PPA and install firefox from apt

```cmd
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update && sudo apt install firefox
```

## Linux File System (Structure)

[Linux-File-System/Structure](https://github.com/twtrubiks/linux-note/tree/master/linux-file-system-structure)

## More Articles

[zsh-tmux-tutorual](https://github.com/twtrubiks/linux-note/tree/master/zsh-tmux-tutorual) - Super useful zsh and tmux.

[zsh-powerlevel10k-tutorual](https://github.com/twtrubiks/linux-note/tree/master/zsh-powerlevel10k-tutorual) - zsh with Powerlevel10k, super beautiful terminal.

[vim-shortcuts](https://github.com/twtrubiks/linux-note/tree/master/vim-shortcuts) - Record vim shortcuts

[imwheel-tutorual](https://github.com/twtrubiks/linux-note/tree/master/imwheel-tutorual) - Improve linux mouse scrolling problem.

[shutter-tutorual](https://github.com/twtrubiks/linux-note/tree/master/shutter-tutorual) - Shutter is an excellent image capture tool.

[systemctl-tutorial](https://github.com/twtrubiks/linux-note/tree/master/systemctl-tutorial) - systemctl command is a system service management command

[crontab-tutorual](https://github.com/twtrubiks/linux-note/tree/master/crontab-tutorual) - Linux Crontab

[mount-disks-at-system-startup-on-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/mount-disks-at-system-startup-on-ubuntu)

[chinese-input-methods-on-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/chinese-input-methods-on-ubuntu) - how to install Chinese input method on ubuntu

[linux-install-locale](https://github.com/twtrubiks/linux-note/tree/master/linux-install-locale) - Linux install Chinese interface (locale)

[hime-tutorial](https://github.com/twtrubiks/linux-note/tree/master/hime-tutorial) - A better Chinese input method in Linux that is more like Microsoft's, hime

[gnome-tweaks](https://github.com/twtrubiks/linux-note/tree/master/gnome-tweaks) - Ubuntu install GNOME Tweak tool

[htop-tutorial](https://github.com/twtrubiks/linux-note/tree/master/htop-tutorial) - htop tutorial :thumbsup:

[fastfetch-tutorial](https://github.com/twtrubiks/linux-note/tree/master/neofetch-tutorial) - command-line system information tool :thumbsup:

[copyq-tutorial](https://github.com/twtrubiks/linux-note/tree/master/copyq-tutorial) - clipboard :thumbsup:

[tldr-tutorial](https://github.com/twtrubiks/linux-note/tree/master/tldr-tutorial) - A better and simpler man pages

[krusader-tutorial](https://github.com/twtrubiks/linux-note/tree/master/krusader-tutorial) - file manager

[fail2ban-tutorial](https://github.com/twtrubiks/linux-note/tree/master/fail2ban-tutorial) - Make the server more secure :smile:

[server related security settings](https://github.com/twtrubiks/linux-note/tree/master/server-set-secure)

[how-to-change-login-lock-background-ubuntu](https://github.com/twtrubiks/linux-note/tree/master/how-to-change-login-lock-background-ubuntu) - Change the background of the Ubuntu login/lock screen

[grub-customizer-tutorial](https://github.com/twtrubiks/linux-note/tree/master/grub-customizer-tutorial) - Install grub-customizer

[enable-ubuntu-remote-tutorial](https://github.com/twtrubiks/linux-note/tree/master/enable-ubuntu-remote-tutorial) - How to enable remote desktop on ubuntu

[linux-nfs-server](https://github.com/twtrubiks/linux-note/tree/master/linux-nfs-server) - How to enable NFS Server on ubuntu

[apache-bench-tutorial](https://github.com/twtrubiks/linux-note/tree/master/apache-bench-tutorial) - Apache Bench (ab) tutorial, a tool for testing server performance.

[subfinder-tutorial](https://github.com/twtrubiks/linux-note/tree/master/subfinder-tutorial) - subfinder finds all subdomains

## Troubleshooting

[fix_could_not_get_lock_dpkg_ubuntu](https://github.com/twtrubiks/linux-note/tree/master/fix_could_not_get_lock_dpkg_ubuntu) - Fix `E: Could not get lock /var/lib/dpkg/lock` Error

[fix_network_manager_bug_ubuntu_18_04](https://github.com/twtrubiks/linux-note/tree/master/fix_network_manager_bug_ubuntu_18_04) - Fix the problem of missing network connection settings in ubuntu 18.04.

## Other

[Windows -> Linux Pros and Cons](https://github.com/twtrubiks/linux-note/tree/master/linux-is-better-than-windows) - Windows -> Linux Pros and Cons

[ubuntu-18-04-on-Lenovo-X1-Carbon-6](https://github.com/twtrubiks/linux-note/tree/master/ubuntu-18-04-on-Lenovo-X1-Carbon-6)

[Install Ubuntu 19.10 via VirtualBox (and some personal thoughts)](https://youtu.be/lI1EMwhW6lE)

[VirtualBox 6.1 Linux Install Guest Addition - Full Screen Tutorial](https://youtu.be/PMw6FPtbbaU)

[alternative-software](https://github.com/twtrubiks/linux-note/tree/master/alternative-software) - windows -> Linux alternative software

[rclone-tutorial](https://github.com/twtrubiks/linux-note/tree/master/rclone-tutorial) - rclone is a great file synchronization management tool

[stacer-tutorial](https://github.com/twtrubiks/linux-note/tree/master/stacer-tutorial) - Linux System Optimizer and Monitoring

[cmatrix-tutorial](https://github.com/twtrubiks/linux-note/tree/master/cmatrix-tutorial) - Super cool and useless cmatrix

[sl-tutorial](https://github.com/twtrubiks/linux-note/tree/master/sl-tutorial) - sl train starts

[linux-tlp-tutorial](https://github.com/twtrubiks/linux-note/tree/master/linux-tlp-tutorial) - Linux Advanced Power Management

[speedtest-cli-tutorial](https://github.com/twtrubiks/linux-note/tree/master/speedtest-cli-tutorial) - Linux speedtest-cli tutorial

[gnirehtet-tutorial-tutorial](https://github.com/twtrubiks/linux-note/tree/master/gnirehtet-tutorial) - Use your computer to let your phone access the internet

[scrcpy-tutorial-tutorial](https://github.com/twtrubiks/linux-note/tree/master/scrcpy-tutorial) - Control your phone with your computer

[variety-tutorual](https://github.com/twtrubiks/linux-note/tree/master/variety-tutorual) - variety automatically changes the desktop

[arandr-tutorual](https://github.com/twtrubiks/linux-note/tree/master/arandr-tutorual) - arandr multi-monitor setting tool

[inotify-tools-tutorial](https://github.com/twtrubiks/linux-note/tree/master/inotify-tools-tutorial) - inotify-tools monitor file changes inotifywait

[ssh-tunneling-tutorial](https://github.com/twtrubiks/linux-note/tree/master/ssh-tunneling-tutorial) - SSH Tunneling Tutorial

[linux-virtualbox-ssh-tutorial](https://github.com/twtrubiks/linux-note/tree/master/linux-virtualbox-ssh-tutorial) - Set up VirtualBox in Linux to play with ssh

[linux-virtualbox-problem](https://github.com/twtrubiks/linux-note/tree/master/linux-virtualbox-problem) - Set up VirtualBox in Linux - Common problems

[QEMU-KVM-tutorial](https://github.com/twtrubiks/linux-note/tree/master/QEMU-KVM-tutorial) - QEMU-KVM (better performance than virtualbox)

[smartmontools-tutorial](https://github.com/twtrubiks/linux-note/tree/master/smartmontools-tutorial) - smartmontools tutorial

[hdparm-tutorial](https://github.com/twtrubiks/linux-note/tree/master/hdparm-tutorial) - hdparm tutorial

[fstrim-ssd-tutorial](https://github.com/twtrubiks/linux-note/tree/master/fstrim-ssd-tutorial) - fstrim ssd tutorial

[exFAT-usb-tutorial](https://github.com/twtrubiks/linux-note/tree/master/exFAT-usb-tutorial) - Let usb store files larger than 4GB tutorial

[Linux Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de)

[Understanding Linux distributions](https://github.com/twtrubiks/linux-note/tree/master/linux-distro)

[KDE setting](https://github.com/twtrubiks/linux-note/tree/master/kde-settings)

[Whisper Local YouTube Subtitle Generation Guide](whisper-tutorial)

## Reference

* [Linux Command Line and Basic Operations Tutorial](https://blog.techbridge.cc/2017/12/23/linux-commnd-line-tutorial/)

## Donation

The articles are all original after my own research and internalization. If they have helped you and you want to encourage me, please buy me a cup of coffee :laughing:

ECPAY (no registration required)

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[Sponsor Payment](http://bit.ly/2F7Jrha)

Opay (registration required)

![alt tag](https://i.imgur.com/LRct9xa.png)

[Sponsor Payment](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## Sponsor List

[Sponsor List](https://github.com/twtrubiks/Thank-you-for-donate)

## License

MIT license
