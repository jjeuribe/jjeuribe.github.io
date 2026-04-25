---
title: HackThisSite - Level 5 - Insecure Password Recovery via Hidden Fields
image:
  path: /assets/2026-04-25-hackthissite-level-5-insecure-password-recovery-via-hidden-fields/1770155647105.jpg
date: 2026-04-25 07:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

Since Sam has gotten wise to all the people who wrote their own forms to get the password, he decided to make his email program a little more secure, or at least that's what he thinks 😏.

In this [challenge](https://www.hackthissite.org/missions/basic/5/), you won’t need Burp Suite, request interception, or anything fancy. The vulnerability is right there in plain sight.

Start by opening your browser’s **DevTools** and inspecting the HTML form responsible for emailing Sam:

![Desktop View](/assets/2026-04-25-hackthissite-level-5-insecure-password-recovery-via-hidden-fields/20260127144342.png)

At this point, you’re probably thinking the same thing I am: why not just change the email address directly in the HTML and have the password sent to *me* instead?, and… yep, that’s exactly what you'll do. Simply edit the `email` field in the HTML to your own address, like this:

![Desktop View](/assets/2026-04-25-hackthissite-level-5-insecure-password-recovery-via-hidden-fields/20260127144632.png)

As simple as that, no interception, no proxy, no extra tools. 

Just submit the form and Sam’s password will show up straight in your email inbox 📬:

![Desktop View](/assets/2026-04-25-hackthissite-level-5-insecure-password-recovery-via-hidden-fields/20260127144856.png)

And just like that, Sam's  password is revealed: `7536c9ac`

## Security Recommendations 

**Always assume the client is malicious**. Anything in HTML can be modified by the user: email addresses, hidden fields, form actions, etc., that's why all of it must be validated and authorized server-side, specially when handling sensitive actions such as password recovery. 

**Passwords** should never be sent via email as this exposes credentials to interception and misuse, instead, secure, time-limited password reset tokens should be implemented. Sensitive business logic must reside on the server, not in client-side code, and proper authentication and recovery workflows should be leveraged to ensure that credentials remain protected even if the frontend is manipulated.
