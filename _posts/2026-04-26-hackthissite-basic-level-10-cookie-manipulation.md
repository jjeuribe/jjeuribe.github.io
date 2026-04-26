---
title: HackThisSite - Basic Level 10 - Cookie Manipulation
image:
  path: assets/2026-04-26-hackthissite-basic-level-10-cookie-manipulation/1771344828808.jpg
  alt: Photo by Wirestock on Freepik
date: 2026-04-26 07:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

This time, Sam went for a more temporary and *"hidden"* approach for user authentication. Unfortunately for him, he forgot one small detail: users weren’t just clicking buttons, they actually knew their way around JavaScript.

When you land on this [challenge](https://www.hackthissite.org/missions/basic/10/), you're greeted with a very simple login form:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-10-cookie-manipulation/20260203221549.png)

After spending some time poking around the page’s source code, you find nothing. No comments, no hints, no suspicious JavaScript, absolutely nothing useful. At first glance, it looks like a dead end.

However, in the description of this challenge you already have a clue: *"something is hidden in the page and is being used for authentication"*. 

From a client-side perspective, the usual suspects that immediately come to my mind are: **cookies 🍪 and tokens 🔑**. 

This [Google resource](https://developer.chrome.com/docs/devtools/application/cookies?hl=es-419) explains how to inspect and manage cookies using Chrome DevTools, worth a quick look if you’re not familiar with it.

With this information in hand, you head straight to the browser’s cookie storage, and sure enough, what you'll find is… interesting:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-10-cookie-manipulation/20260203224103.png)

One line immediately stands out:

```
level10_authorized=no
```

That raises a very important question: *Is the web server trusting this cookie to determine whether a user is authorized, without requiring a password?*. Well, there’s only one way to find that out. Change the value to:

```
level10_authorized=yes
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-10-cookie-manipulation/20260203224644.png)

Now submit the form (use a random password, or don’t enter one at all). The result?:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-10-cookie-manipulation/20260203224830.png)

Access granted. 🎉

And just like that, you’re in.  Well done, Sam trusted the client, and the client lied.

## Security Recommendations

Authentication and authorization decisions should **never be trusted to the client side**, as cookies, local storage, and JavaScript values can be easily modified by an attacker. All access control logic must be enforced **server-side**. 

Sensitive cookies should be marked with appropriate attributes such as `HttpOnly`, `Secure`, and `SameSite` to reduce the risk of client-side manipulation and cross-site attacks.

Implementing proper session management, token signing (like HMAC or JWT with verification), and regular security testing can significantly reduce the risk of authentication bypass vulnerabilities.
