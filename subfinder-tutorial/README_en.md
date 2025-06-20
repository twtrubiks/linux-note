[中文版](README.md)

# subfinder Tutorial

Find all subdomains using subfinder.

## Description

GitHub link: [https://github.com/projectdiscovery/subfinder](https://github.com/projectdiscovery/subfinder)

This tutorial uses the Docker installation method.

```cmd
docker pull projectdiscovery/subfinder:latest
```

Then you can use the following command.

For example, if I want to find all subdomains of google.com:

```cmd
docker run projectdiscovery/subfinder:latest -d google.com
```

You can also find all active domains (but this does not mean the website is functioning correctly):

```cmd
docker run projectdiscovery/subfinder:latest -d google.com -nW
```

For more command instructions, please refer to the GitHub repository.

If you want to check the IP of a domain after finding it, you can use the following command:

```cmd
nslookup www.google.com
