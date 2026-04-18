---
title: LetsDefend - Http Basic Auth
image:
  path: /assets/2026-04-03-letsdefend-http-basic-auth/0_RS80yiblnNsvtfeS.webp
  alt: Photo by iMattSmart on Unsplash
date: 2026-04-03 07:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, letsdefendio, soc-analyst, wireshark]     # TAG names should always be lowercase
---

During this investigation you'll be gathering information about the client and server. To do so, you'll work with a provided `.pcap` file to help answer the following questions. Without further ado, let’s get started.

You can find the challenge on the official [LetsDefend website](https://app.letsdefend.io/challenge/investigate-web-attack)

## How many HTTP GET requests are in pcap?

To answer this question you just need to filter by HTTP protocol and HTTP method, that is: 

```
http.request.method == "GET"
```

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511122915.png)

So, the answer is: `5 GET requests`

## What is the server operating system?

To fetch this information, I usually first ensure that the server responds successfully. Here’s a simple display filter to help you visualize it:

```
http.response.code == 200
```

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511115642.png)

The information you're being asked can be found in the `Server` response header, as shown in the picture. 

The answer for this question is: `FreeBSD`

## What is the name and version of the web server software?

This information can be found in the same `Server` response header. There's no need to create a new filter:

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511120236.png)

The answer for this question is: `Apache/2.2.15`

## What is the version of OpenSSL running on the server?

In the same way, this information can be found in the `Server` response header:

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511120220.png)

So, the answer is: `OpenSSL/0.9.8n`

## What is the client's user-agent information?

You can easily retrieve this information from any GET requests by simply checking the `User-Agent` header:

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511120539.png)

Yep. That whole-ass line is the client's user-agent information. 

The answer for this question is: `Lynx/2.8.7rel.1 libwww-FM/2.14 SSL-MM/1.4.1 OpenSSL/0.9.8n`

## What is the username used for Basic Authentication?

As you may already know, **Basic Authentication** is a simple but widely used HTTP authentication scheme in which you, as the client, provide your username and password to the server by including an encoded string in the `Authorization` header.

To answer this question you just need to follow the HTTP conversation between the client and the server:

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511121638.png)

As you can see, when you sent your first `GET` request, the server required authentication. You then provided the correct credentials using the **Basic Authentication** scheme in a subsequent HTTP GET request, which can be inspected to reveal the credentials. After receiving and processing your credentials, the server responded with a successful authentication.

Therefore, the answer for this question is: `webadmin`

## What is the password used for Basic Authentication?

To answer this question, you don't really have to do much but to stay on the same screen:

![Desktop View](/assets/2026-04-03-letsdefend-http-basic-auth/20250511121724.png)

Just inspect the header of the same `GET` request, and you'll get the answer.

So, the answer for this question is: `W3b4Dm1n`
