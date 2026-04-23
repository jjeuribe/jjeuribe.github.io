---
title: LetsDefend - Phishing Email
image:
  path: /assets/2026-04-23-letsdefend-phishing-email/0_uoIYwdTbHKnECLjx.webp
  alt: Photo by CHUTTERSNAP on Unsplash
date: 2026-04-23 08:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, letsdefendio, soc-analyst, phishing-analysis, virus-total]     # TAG names should always be lowercase
---

**Phishing emails** are one of the most common attack vectors in cybersecurity, and detecting them requires careful analysis. In this challenge, you’re tasked with dissecting a suspicious email file to uncover the hidden signs of phishing and confirm whether it’s an attack. Use any tools or knowledge at your disposal. 

Ready? Let’s begin! 🕵️‍♂️ 

First, start by extracting the file from the ZIP archive you were provided:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128155929.png)

Next, navigate into the extracted folder. Inside, you’ll find the `paypal.eml` file:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128160018.png)

To inspect the email, open it with **Mozilla Thunderbird** for a proper email client view:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128175749.png)

For a closer look at the raw contents, open it in **Notepad++** as well:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128175922.png)

You now have everything you need to tackle this challenge. So, let's get to it!.

## What is the return path of the email?

The `Return-Path` contains the email address where **bounced messages** (undeliverable emails) are sent. For example, if you send an email and it cannot be delivered, the bounce notification will be sent to this address.

You can find this information in the email header:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128180212.png)

So, the answer is: `bounce@rjttznyzjjzydnillquh.designclub.uk.com`

## What is the domain name of the url in this mail?

Inside the email, you’ll notice a big blue action button. If you hover over it, you’ll see a URL like this:

```
https://storage.googleapis.com/hqyoqzatqthj/aemmfcylvxeo.html#QORHNZC44FT4.QORHNZC44FT4?dYCTywccxr3jcxxrmcdcKBdmc5D6qfcJVcbbb4M
```

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128164326.png)

Looking closely, the domain name in that URL is: `storage.googleapis.com`

## Is the domain mentioned in the previous question suspicious?

A link **pointing to `storage.googleapis.com` can be suspicious**, but in can also be legitimate, it all depends on the context. In my opinion, an action button should never link directly to cloud storage. However, let's see what VirusTotal has to say about this. 

Copy and paste the previous URL up to the `.html` page: 

```
https://storage.googleapis.com/hqyoqzatqthj/aemmfcylvxeo.html
```

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128181021.png)

A few vendors flag this URL as malicious, and the *Community Score* is negative. Let's check the VirusTotal Community feedback:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128181208.png)

Most community members flag this as a phishing attempt. So, indeed, this domain is suspicious.

Therefore, the answer for this question is: `yes`

## What is the body SHA-256 of the domain?

To answer this question, take the domain as follows and paste it into VirusTotal: 

```
https://storage.googleapis.com/
```

Next, go to the **DETAILS** tab and scroll down until you find the *Body SHA-256* label:

![Desktop View](/assets/2026-04-23-letsdefend-phishing-email/20251128182918.png)

The answer is: `13945ecc33afee74ac7f72e1d5bb73050894356c4bf63d02a1a53e76830567f5`

## Is this email a phishing email?

After carefully analyzing the headers, inspecting the links, and consulting VirusTotal, it’s clear that this email is indeed a phishing attempt. Every detail, from the suspicious `Return-Path` to the questionable domain and the negative community feedback, points to malicious intent.

So, the answer to this question is: `yes`

This challenge highlighted the importance of examining every element in an email before acting 🧐. With a systematic approach and the right tools 🛠️, you can spot phishing attacks and protect yourself from falling victim. Remember: **in cybersecurity, vigilance is your best defense 🛡️**.
