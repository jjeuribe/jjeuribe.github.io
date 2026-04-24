---
title: HackTheBox - Understanding Log Sources and Investigating With Splunk, Skills Assessment
image:
  path: /assets/2026-04-24-hackthebox-understanding-log-sources-and-investigating-with-splunk/0_X5MJB3aug5cerkCx.webp
  alt: Photo by Andres Siimon on Unsplash
date: 2026-04-24 07:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, hackthebox, soc-analyst, splunk, windows-security, windows-event-logs]     # TAG names should always be lowercase
---

This lab is part of the *Skills Assessment* for [Understanding Log Sources and Investigating With Splunk](https://academy.hackthebox.com/course/preview/understanding-log-sources--investigating-with-splunk). In this article, I’ll guide you through solving each question, but I encourage you to try the challenges yourself first , that's the best way to learn. Ready? Let’s do this 🚀

## Navigate to `http://[Target IP]:8000`, open the "Search & Reporting" application, and find through SPL searches against all data the process that created remote threads in rundll32.exe. Answer format: `_.exe`

This challenge wants you to identify the process that is performing thread injection into `rundll32.exe`. In Sysmon terms, this means you should check **Event ID 8 - CreateRemoteThread** events and pay special attention to the `SourceImage` and `TargetImage` fields: 

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=8 TargetImage="*\\rundll32.exe"
| stats count by SourceImage, TargetImage
```

![Desktop View](/assets/2026-04-24-hackthebox-understanding-log-sources-and-investigating-with-splunk/20251016133305.png)

That's it!, as simple as that. The injector process is: `randomfile.exe`

## Navigate to `http://[Target IP]:8000`, open the "Search & Reporting" application, and find through SPL searches against all data the process that started the infection. Answer format: `_.exe`

Alright, this question is one of those classic blue-team traps meant to test your analytical reasoning, not just whether you can query things. 

My first approach was to find the *first process in the chain*, that is, *the first parent process*, *the initial dropper*, you name it. However, that's not what they are asking for. They're not talking about the first process in the tree. This question is asking you for the binary where the infection actually started. 

So, even though `randomfile.exe` was the injector, the malicious code itself begins running inside `rundll32.exe`

That's why the answer for this question is `rundll32.exe`

Alright, that's all for now! I hope this walkthrough has helped you out. Keep practicing, and happy hunting! 🕵️‍♂️

