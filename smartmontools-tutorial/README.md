# smartmontools 教學

檢查硬碟狀況

安裝指令

```cmd
sudo apt install smartmontools
```

可以先用 `df -h` 查看硬碟代號,

然後執行

```cmd
sudo smartctl --all /dev/sda
```

`-a` `--all` 代表 Show all SMART information for device

更多指令用法可使用 `smartctl --help` 查詢.
