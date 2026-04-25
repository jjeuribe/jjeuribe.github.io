---
title: HackThisSite - Basic Level 3 - Publicly Accessible Sensitive Files
image:
  path: /assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/0_SRypJTHSNwEHzUTw.webp
  alt: Photo by Maksym Kaharlytskyi on Unsplash
date: 2026-04-25 05:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

Alright. This time Network Security Sam remembered to upload the password file, but there were deeper problems than that.

There isn’t much information provided for this [challenge](https://www.hackthissite.org/missions/basic/3/), other than the fact that there’s now a file containing an unencrypted password and this screen:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/20260130144309.png)

At this point, spending some time poking around the page’s source code to see if anything interesting pops up is a solid move. Trust me, it often pays off 😉

After a bit of digging, you end up looking at the structure of the login form:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/20260127123140.png)

From this, you can infer that when you press the **Submit** button, a POST request is sent to the following endpoint for processing:

```
https://www.hackthissite.org/missions/basic/3/missions/basic/3/index.php
```

Here you can see the POST request details:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/20260127124803.png)

And the POST request payload: 

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/20260127125333.png)

Now here’s the real question: why is a string called `password.php` being sent as part of the form data? Is this the password file Sam uploaded that contains the real password?, and if so, how can you access to the content of that `.php` file?. 
 
 Well, first, you need to figure out whether that file is publicly accessible. My guess is that the `index.php` script opens `password.php`, reads the password from it, and compares it against user's input. If that’s the case, then `password.php` is probably located relative to `/missions/basic/3/`, something like this:

```
https://www.hackthissite.org/missions/basic/3/password.php
```

Paste that URL into your browser and check the result:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-3-publicly-accessible-sensitive-files/20260127135410.png)

Voilà 🎯, your assumption was correct. There’s Sam’s password: `977f405d`

As you can see, the password was stored in an external file that’s publicly accessible. **Attackers love these kinds of mistakes.** 😬

## Security Recommendations

Storing passwords in publicly accessible files is a major security risk and should be avoided. Passwords should **never** be hardcoded or saved in web-accessible directories. Instead, they should be stored securely in a server-side database with strong **hashing** (e.g, bcrypt) and proper access controls. 

Any script that process sensitive data, (like `index.php` in this scenario), should **sanitize and validate all user input** to prevent code injection and unauthorized file access. Additionally, sensitive files should be placed **outside the web root** or protected with proper server permissions to ensure they cannot be directly accessed via a URL. 

Regularly reviewing file permissions and server logs can also help detect misconfigurations or attempted attacks early.
