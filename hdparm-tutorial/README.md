# hdparm 教學

Discard unused blocks on a mounted filesystem.

優化 ssd, 丟棄不必要的 blocks.

通常 linux 預設都有這個指令

```cmd
sudo fstrim --help
```
然後執行以下指令

```cmd
sudo fstrim --fstab --verbose --quiet
```

`-A` `--fstab` 代表 trim all supported mounted filesystems from /etc/fstab

`-v` `--verbose` 代表 print number of discarded bytes

`--quiet` 代表 suppress error messages

執行完之後就會和你說丟棄掉多少不必要的 blocks 了
