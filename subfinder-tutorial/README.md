# subfinder 教學

透過 subfinder 找出全部的子網域,

## 說明

github 連結 [https://github.com/projectdiscovery/subfinder](https://github.com/projectdiscovery/subfinder)

這邊使用 docker 的安裝方法

```cmd
docker pull projectdiscovery/subfinder:latest
```

接著就可以使用, 以下指令,

例如我想要找出全部的 google.com 的子網域

```cmd
docker run projectdiscovery/subfinder:latest -d google.com
```

也可以找出全部還活著的 domain (但不代表網站是正常的)

```cmd
docker run projectdiscovery/subfinder:latest -d google.com -nW
```

更多的指令說明可參考 gihutb

如果找出網域後, 想再看查看該網域的 ip, 可以使用以下的指令,

```cmd
nslookup www.google.com
```

