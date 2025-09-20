---
title: iframe hacking
published: 2025-09-19
description: 'Using <iframe> to hacking in client-side'
image: "guide/gokuegohan.jpg"
tags: [web-vulns, client-side]
category: 'WebSec'
draft: false 
---

Hello guys, just imagine using the tag `<iframe>` to hacking? Let's see some use cases.

## Tha iframe tag

The `<iframe>` is an HTML tag used to specify an inline frame. An inline frame is used to embed another document within the current HTML document. The `<iframe>` tag allows an external page to be rendered inside another, creating a "parent-child" relationship between different domains. 

## Interesting Attributes

Some attributes are interesting to use when looking for vulnerabilities:

- `src=` → Defines the URL of the page to be loaded inside the iframe.
- `srcdoc=` → Allows embedding HTML code directly into the attribute, instead of loading an external URL. The content of srcdoc takes precedence over src.
- `name=` → Gives a name to the iframe, which can be used to reference it via JavaScript `window.frames['name']` or as a target for forms and links `<form target="iframe_name">`.

## Exploitation Examples for These Attributes:

```javascript
<iframe src="<https://malicious-site.com/fake-page.html>"></iframe>
<iframe src="data:text/html,<script>alert(document.domain)</script>"></iframe>
<iframe src="javascript:alert(document.domain)"></iframe>
<iframe srcdoc="[malicious_html]"></iframe>
<iframe srcdoc="<svg onload=alert('XSS_in_parent_origin!')>"></iframe>
<iframe srcdoc="<script>alert(document.domain)</script>"></iframe>
```

## BROWSER Security Mechanisms

**Same-Origin Policy (SOP)**: Prevents a document or script loaded from one origin (e.g., `https://attacker-site.com`) from reading or modifying data from another origin (e.g., `https://bank.com`). An origin is the combination of protocol (http/https), domain, and port.

```markdown
How does it affect <iframe> tag? If page A embeds page B from a different origin in an iframe, page A cannot access the content of iframe B parent.document.getElementById('password') will fail. Similarly, page B in the iframe cannot access the content of page A.
```

**X-Frame-Options (XFO)**: This is an HTTP response header, an instruction from the server to the browser. It tells the browser whether the page can be rendered inside a `<frame>`, `<iframe>`, `<embed>`, or `<object>`.

```markdown
How does it affect <iframe>? It is the main defense against Clickjacking.
`X-Frame-Options: DENY` = No page can embed this page in an iframe.
`X-Frame-Options: SAMEORIGIN` = Only pages from the same origin can embed it.
If X-Frame-Options is `MISSING`: Is it vulnerable? Only if a Content-Security-Policy with frame-ancestors is also absent.
```

**Content Security Policy (CSP)**: CSP is the evolution of XFO. It is a much more powerful and flexible HTTP header. It allows a site administrator to define a "whitelist" of trusted content sources.

```markdown
How does it affect <iframe> tag? Mainly through two directives:
`frame-ancestors`: Replaces XFO. It defines who can embed the page. frame-ancestors 'none' is the most secure. frame-ancestors 'self' only allows embedding from the same domain.
`frame-src (or child-src)`: Defines which URLs the current page is allowed to load within an iframe.
```

**sandbox Attribute**: This is a policy you apply directly to the `<iframe>` tag. It places the iframe's content in a "maximum security mode," disabling scripts, forms, pop-ups, and other dangerous features.

```markdown
How does it affect <iframe> tag? It is a defense-in-depth measure. Even if you manage to inject an iframe pointing to a malicious page, the sandbox attribute can neutralize its payload.
```

**Permissions Policy (formerly Feature Policy)**: This policy (sent via HTTP header or the allow attribute in the iframe) controls access to powerful browser features. It defines whether the content can use APIs like the camera, microphone, geolocation, full-screen mode, etc.

```markdown
How does it affect <iframe> tag? It limits what an iframe, even a malicious one, can do.
```

## Video

<iframe width="100%" height="468" src="https://www.youtube.com/embed/eljAcnMRVf4" title="Abuso de iframes por um hacker do lado do cliente (Ep. 119)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## Links

https://www.intigriti.com/researchers/hackademy/clickjacking
https://www.invicti.com/blog/web-security/frame-injection-attacks/
https://hackerone.com/reports/2212950
https://hackerone.com/reports/1200770
https://hackerone.com/reports/2964441
https://hackerone.com/reports/1166766
https://hackerone.com/reports/3287060
https://www.youtube.com/watch?v=MYSiVEa29m0
https://www.youtube.com/watch?v=_tz0O5-cndE
https://www.youtube.com/watch?v=254qpAOvzOA
https://www.youtube.com/watch?v=eljAcnMRVf4
https://www.youtube.com/watch?v=1J08mouZIk4
https://www.w3schools.com/tags/tag_iframe.ASP