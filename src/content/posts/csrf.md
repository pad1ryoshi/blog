---
title: Cross Site Request Forgery
published: 2025-04-23
description: 'Notes abouts CSRF attacks'
image: "guide/seiya.jpg"
tags: [csrf, web-vulns]
category: 'WebSec'
draft: false 
---

Web vulnerability that allows the attacker to induce the end user to perform unwanted actions. This vulnerability allows the attacker to partially bypass the same-origin policy (SOP), which prevents different websites from interfering with each other.

Example email exchange: 
https://sitevulneravel.com/email/trocar?email=ownado@evil.net

Impact:
The impact of this vulnerability will depend on the context but, if successful, the attacker will cause the victim to carry out actions unintentionally.

### Conditions in an useful CSRF

```markdown
1. Revelant action (CSRF in logout is a meme fr)
2. Cookie-based session handling (be aware about JWT)
3. No unpredictable request parameters (if an attacker needs to know the value of the parameter, bye)
```

### How to find

```markdown
- Remove CSRF token from request, replace with random value or blank space
- Change POST to GET
- Replace CSRF token with an already used token
- Bypass regex
- Get a token by request and call manually
- Extract the token with XSS or HTML injection
```

### Some bypass

```markdown
CSRF tokens:
- Change HTTP request's methods
- Remove the entire token parameter 
- Use a valid token and feed that token to the victim
- Try to find any behavior that allows to set a cookie in a victim's browser
- Try to find any functionality that contains cookie's setting's

SameSite cookies:
- If the value is `SameSite=None` it's worth investigating whether it's of any use
- Lax bypass via method override/http verb tampering
- Strict bypass via on-site gadget (like CSPT + OPEN REDIRECT to CSRF action)
```

### How to prevent CSRF

```markdown
. Use CSRF tokens
. Use Strict or Lax SameSite cookie restrictions
. Be wary of cross-origin, same-site attacks
```

### Youtube
<iframe width="100%" height="468" src="https://www.youtube.com/embed/eWEgUcHPle0" title="Cross-Site Request Forgery (CSRF) Explained" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Resources

[OWASP CSRF Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
[Web Academy](https://portswigger.net/web-security/csrf)
[caon.io 🐐](https://caon.io/exploitation/vulnerability/csrf/)