---
title: HackThisSite - Basic Level 7 - Command Injection
image:
  path: /assets/2026-04-25-hackthissite-basic-level-7-command-injection/1770662562771.jpg
  alt: Photo by Mohammad Rahmani on Unsplash
date: 2026-04-25 09:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

In this [challenge](https://www.hackthissite.org/missions/basic/7/), Sam stored the unencrypted password in a file in this very directory: 

```
https://www.hackthissite.org/missions/basic/7/
```

And in other unrelated news, Sam has also created a script that displays the output of the UNIX `cal` command. So, when a user inputs a year:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-7-command-injection/20260128140319.png)

Sam's backend script executes the `cal` command, and the following result is produced: 

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-7-command-injection/20260128140426.png)

My guess is that the highlighted Perl (`.pl`) script is the one executing the `cal` command. If the form input text receiving the year isn't properly sanitized and validated, it could introduce some sort of injection attack. Let's test this hypothesis out by entering the following command directly in the input text:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-7-command-injection/20260128141113.png)

And see the result: 

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-7-command-injection/20260128141821.png)

Yep, a classical **command injection** scenario. You can inspect the contents of the files exposed in the output, however the one containing Sam's unencrypted password is `k1kh31b1n55h.php`:  

```
https://www.hackthissite.org/missions/basic/7/k1kh31b1n55h.php
```

The result is:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-7-command-injection/20260128142156.png)

So, Sam's password is revealed once again: `cc54ac24`

## Security Recommendations

Any input received from users must be strictly sanitized and validated to ensure it contains only expected values. On top of this, developers should avoid invoking shell commands whenever possible, and instead use native language libraries or safer APIs, which greatly reduce the risk of injection attacks.

Additionally, applications should maintain a clear separation between application logic and the operating system. Backend scripts should run with the least privileges required, ensuring that even if an injection occurs, the attacker cannot read sensitive files or traverse directories freely in the system.
