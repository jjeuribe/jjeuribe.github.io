---
title: LetsDefend - Shellshock Attack
image:
  path: /assets/2026-04-22-letsdefend-shellshock-attack/0_qMsgqDwFZUX5GUJK.webp
  alt: Photo by Gabriel Heinzer on Unsplash
date: 2026-04-22 08:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, letsdefendio, soc-analyst, wireshark, bash]     # TAG names should always be lowercase
---

A **Shellshock** attack is a security vulnerability that affects the **Bash shell**. It was discovered in **September 2014** and it is tracked as **CVE-2014-6271**. 

The vulnerability stems from incorrect parsing of functions exported in environment variables. For example: 

```bash
> env x='() { :; }; echo Vulnerable' 
```

If the system is vulnerable, you'd see the following: 

```
Vulnerable
```

This shouldn't have happened, bash didn't stop parsing at the function definition and executed a trailing payload. An adversary could exploit this buggy behavior to run arbitrary commands on the system. But how can this be done?.

Well, there are several ways, but the classic attack vector is a CGI web server. This type of web server often runs bash scripts and exports request data (headers, query strings, etc.) into environment variables. A good example is Apache web server, which as built-in support for CGI. 

Take the following HTTP request as an example: 

```
GET / HTTP/1.1
Host: yourcompany.com
User-Agent: () { :;}; echo "NS:" $(</etc/passwd)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
```

Let's say this request hits a CGI endpoint, Apache then would have to fork a new process and run the interpreter defined by the script's shebang (for example`#!/bin/bash`). The interpreter then will inherit and parse the environment variables Apache exported for the request. If a vulnerable Bash interpreter processes the `User-Agent` value above, the attacker could read the `/etc/passwd` file.

So yes, as you can see, a Shellshock attack essentially allows **Remote Code Execution (RCE)** on the target.

Now that you're familiar with what a Shellshock attack is all about, you're ready to answer the following questions. 

First things first, open the provided `shellshock.pcap` file and filter by the `http` protocol:

![Desktop View](/assets/2026-04-22-letsdefend-shellshock-attack/20251113150809.png)

You'll notice there are only two packets, and that's all you need to tackle this challenge. Without further ado, let’s get started!

## What is the server operating system?

To answer this question simply check what the server has to say in the response. You'll find that information in the second packet, specifically in the HTTP header `Server`: 

![Desktop View](/assets/2026-04-22-letsdefend-shellshock-attack/20251113151043.png)

The answer for this question is: `Ubuntu`

## What is the application server and version running on the target system?

This one is a no-brainer. The web server and its version is also in the same HTTP header `Server`:

![Desktop View](/assets/2026-04-22-letsdefend-shellshock-attack/20251113151403.png)

So, yeah. The answer is: `Apache/2.2.22`

## What is the exact command that the attacker wants to run on the target server?

This is the icing on the cake. Using what you know about Shellshock, find the HTTP header that contains the malicious payload that lets the attacker run commands on the server.

I know what you’re thinking, let’s inspect the HTTP request and see what it can reveal:

![Desktop View](/assets/2026-04-22-letsdefend-shellshock-attack/20251113152426.png)

Yes, you got it!. If the CGI endpoint `/exploitable.cgi` processes the `User-Agent` value in a vulnerable environment, Bash can be tricked into executing `/bin/ping -c1 10.246.50.2`. 

The answer for this question is: `/bin/ping -c1 10.246.50.2`

And that's basically it. I hope this write-up made things clearer and helped you tackle this LetsDefend challenge with confidence. Thanks for reading, and see you next time!
