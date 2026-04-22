---
title: LetsDefend - Malicious Doc
image:
  path: /assets/2026-04-22-letsdefend-malicious-doc/0_x6AqtG765hkFof6V.webp
  alt: Photo by Rubaitul Azad on Unsplash
date: 2026-04-22 09:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, letsdefendio, soc-analyst, malware-analysis, virus-total]     # TAG names should always be lowercase
---

An **RTF attack** is a malware **delivery technique** where an attacker sends a specially crafted `.rtf` file designed to exploit a vulnerability in Microsoft Office, usually Word.

Attackers love RTF because through a weaponized Word document they can: 

* Carry malicious exploit code. 
* Embed an OLE object, an Equation Editor component, or an ActiveX control that triggers a vulnerability. 
* Include a downloader to retrieve additional payloads. 
* Bypass email security filters, etc.

The good news for attackers is that Word can process and execute these embedded artifacts **even when macros are disabled**. So, when a victim opens the malicious `.rtf` file in Word, the embedded vulnerability is triggered, leading to shellcode execution, malware being dropped, or even a reverse shell being opened. 

You can detect whether a `.doc` fie is actually an RTF file before opening it. RTF files always start with a very specific ASCII signature: 

```
{\rtf
```

So, you can quickly test a suspicious Word file using the following command: 

```bash
> file factura.doc
```

If it's actually an RTF file, you'll see something like: 

```
factura.doc: Rich Text Format data, unknown version
```

Otherwise, if it's a genuine binary Word document, you'll see: 

```
Composite Document File V2 Document
Microsoft Word Document
```

Keep in mind, though, that RTF files aren't inherently harmful, the danger comes only from the exploit they carry. 

Alright, long story short. Let's extract the malicious Word document from the provided ZIP file:

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117115859.png)

Now, proceed to get the fingerprint of the suspicious file `factura.doc` with the following command: 

```bash
> sha256sum factura.doc 
```

You'll get: 

```
5a31c77293af2920d7020d5d0236691adcea2c57c2716658ce118a5cba9d4913  factura.doc
```

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117120250.png)

Copy that hash and paste it into VirusTotal, I'm leaving you a link with the hash already pasted in: [virus-total-facturadoc](https://www.virustotal.com/gui/file/5a31c77293af2920d7020d5d0236691adcea2c57c2716658ce118a5cba9d4913).

Now, you have everything you need to start answering this challenge's questions. Let's get to it!. 

## What type of exploit is running as a result of the relevant file running on the victim machine?

Alright, to answer this question, just look for all the `rtf.` entries in the following screen:

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117154025.png)

The entry that got me the correct answer was this: 

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117154115.png)

I find this question poorly worded, it should be something like: what type of malicious document technique is being used to deliver the exploit?. This is because RTF isn't the exploit, it's just the delivery container, but, anyway 🤷‍♂️.

The answer for this question is: `rtf.exploit`

## What is the relevant Exploit CVE code obtained as a result of the analysis?

You're being asked for the CVE (Common Vulnerabilities & Exposures) identifier for this security flaw. You already have the answer based on the previous question's response (just replace the underscore (`_`) characters with hyphens (`-`)):

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117154925.png)

So the answer is: `cve-2017-11882`

## What is the name of the malicious software downloaded from the internet as a result of the file running?

Here, you need to identify the malicious software downloaded by the malicious document from the internet, most likely via an HTTP or HTTPS request. You can find that information in the **_Network Communication_** section on the **BEHAVIOR** page in VirusTotal:

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251117160206.png)

The downloaded malicious software is: `jan2.exe`

## What is the IP address and port information it communicates with?

In that same **_Network Communication_** section, you can also find the IP address and port this malicious file is communicating with.

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251118113623.png)

You can see the resolved IP address for the `seed-bc.com` domain, and just below it, the port used for the communication.

The answer for this question is: `185.36.74.48:80`

## What is the exe name it drops to disk after it runs?

On the same **BEHAVIOR** page, simply scroll down until you find the **_Files Dropped_** section. There's only one entry with an `.exe` file, that's your answer:

![Desktop View](/assets/2026-04-22-letsdefend-malicious-doc/20251118114251.png)

Hence, the answer for this question is: `aro.exe`

Alright! 🎉 Throughout this challenge, you’ve seen how a simple RTF file can deliver an entire attack chain. Now you know how to recognize this type of threat, and I hope this walkthrough helped you out and sparked your interest.

Keep exploring, keep practicing, and stay sharp in your investigations. 🔍. See you next time! 👋
