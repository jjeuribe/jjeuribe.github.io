---
title: HackThisSite - Basic Level 4 - Insecure Password Recovery via Request Manipulation
image:
  path: /assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/0_3JKQGBm7f4Ricv6e.webp
  alt: Photo by Towfiqu barbhuiya on Unsplash
date: 2026-04-25 06:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

This time, Sam had the *brilliant* idea 💡 to write a script that would email his password to him in case he forgot it.

In this [challenge](https://www.hackthissite.org/missions/basic/4/), you're presented with the following screen:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127140842.png)

When you hit the button "Sent password to Sam", you can immediately see what’s happening using your DevTools:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127141040.png)

It’s just a simple POST request to the following endpoint:

```
https://www.hackthissite.org/missions/basic/4/level4.php
```

With a payload that looks like this:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127141323.png)

From here, it’s game over. You can intercept that POST request using for example Burp Suite:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127142734.png)

And then, simply change the email value to your own:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127143059.png)

Forward the request, and Sam’s password will land straight in your email inbox 📬:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/20260127143311.png)

So there it is, the password is: `3c752fe0`

## Security Recommendations

Credentials should never be hardcoded or sent via email. Instead, applications must use secure, **single‑use password reset links for account recovery**.

Sensitive actions like account recovery must require proper authentication and enforce strict server‑side validation. Additionally, password-related endpoints should be protected with rate limiting and monitoring to detect abuse, and passwords must always be stored using strong hashing and salting algorithms to prevent disclosure even if the system is compromised.
