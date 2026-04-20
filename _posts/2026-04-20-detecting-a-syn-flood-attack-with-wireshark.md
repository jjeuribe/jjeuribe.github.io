---
title: Detecting a SYN Flood Attack with Wireshark
image:
  path: /assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/0_t8z3KD5Kg1HIlr7a.webp
date: 2026-04-20 08:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, network, soc-analyst, incident-response, wireshark]     # TAG names should always be lowercase
---

A **SYN Flood** attack is a *Denial of Service (DoS) attack* that exploits the way the TCP Three-way Handshake works by flooding a target with a large number of **TCP SYN** packets. The goal is to exhaust the target's resources, blocking this way any legitimate connection to it.

You see, in a normal TCP Three-way Handshake between two devices, the first device sends a `SYN` packet to show its intention to establish a connection with a second device. The second device replies with a `SYN-ACK` packet, confirming the receipt of the first's device request and reserving memory to store the TCP state for the final step of the handshake. Finally, when this second device receives the final `ACK` from the first device, the handshake is completed and the TCP connection is established successfully:

![Desktop View](/assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/20250919125802.png)

Malicious actors can abuse this behavior by never sending the final `ACK` that completes the handshake, leaving the connection hanging and the target stuck in a **half-open state**, wasting memory resources along the way until it eventually times out. The catch is, if the number of **half-open connections** keep piling up, the target's resources will get drained causing it to crash or become unresponsive:

![Desktop View](/assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/20250919130609.png)

## Don’t Get Flooded, Detect SYN Attacks Early

You first red flag should be multiple `SYN` packets going to the same destination. You can identify this pattern with the following display filter: 

```
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

That will get you only `SYN` packets that are sent by hosts that want to initiate a connection. You can use this as your first move when it comes to detecting potential SYN flood attacks.

![Desktop View](/assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/20250914181109.png)

As you can see, there's a high volume of `SYN` packets targeting the same destination host `10.128.0.2:80` within a very short time. This pattern suggests automated and coordinated activity, which is a strong indicator of a SYN flood attack.

You can get a quick aggregated overview of that behavior by navigating to `Statistics → Conversations`:

![Desktop View](/assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/20250915130541.png)

As shown, the host `10.0.0.2` has sent 1,658 `SYN` packets to the target host `10.128.0.2`, which represents one-third of the total traffic.

## Flood or Flop? Checking SYN Attack Success

Now that you've flagged a potential SYN flood attack, the next step is to confirm the success of it. 

You can tell a SYN flood attack is being effective when a target host is allocating resources after replying with a `SYN-ACK` packet but never receiving the final `ACK` required to establish the corresponding TCP connection. This creates what's known as a **half-open TCP connection**, essentially, a handshake that never completes, leaving the target host stuck in a **SYN-RECEIVED** state until it times out. 

Wireshark by itself won't automatically flag **half-open TCP connections**, that's something you must investigate and interpret yourself. A practical way to do this is by using the **I/O Graphs** feature that lets you visualize network traffic over time, allowing you to observe specific traffic patterns.

Just head to `Statistics → I/O Graphs` and add the following filters:

![Desktop View](/assets/2026-04-20-detecting-a-syn-flood-attack-with-wireshark/20250918163244.png)

* `tcp.flags.syn == 1 && tcp.flags.ack == 0 && (ip.src == 10.0.0.2 && ip.dst == 10.128.0.2)`: It shows initial SYN packets coming from `10.0.0.2` to `10.128.0.2`. Useful for spotting SYN floods or handshakes failures. 
* `tcp.flags.syn == 1 && tcp.flags.ack == 1 && (ip.src == 10.128.0.2 && ip.dst == 10.0.0.2)`: It shows SYN-ACK packets coming from `10.128.0.2` to `10.0.0.2`. Useful to confirm whether the target host is alive and properly responding to SYN requests. 
* `tcp.flags.ack == 1 && tcp.flags.syn == 0 && (ip.src == 10.0.0.2 && ip.dst == 10.128.0.2)`: It shows pure ACK packets from `10.0.0.2` to `10.128.0.2`. You can use it to confirm whether source hosts are completing handshakes. 
* `tcp.analysis.retransmission`: It shows retransmitted packets. 
* `tcp.analysis.flags`: It is used to flag any TCP packet that looks "weird", like it was retransmitted, arrived out of order, or any other issue. It helps you to spot network problems quickly.

You see, under normal conditions, `SYN`s and `SYN-ACK`s should be in a 1:1 ratio, that is, one `SYN` in, one `SYN-ACK` out. During a SYN flood attack, you'll typically see more `SYN`s than `SYN-ACK`s because a lot of those `SYN`s are simply fake. However, when looking at the graph you can notice more `SYN-ACK`s than `SYN`s along with repeated retransmissions, that usually means the target host is retrying its `SYN-ACK` responses maybe because it hasn't received the final `ACK`.  In other words, the target host is stuck in a **SYN-RECEIVED** state, retransmitting `SYN-ACK` packets over and over. 

That's exactly the condition of **half-open connections**, no handshakes are completing. If these half-open connections keep piling up, the target host may eventually crash or stop responding, resulting in a successful denial of service.

In short, always keep an eye out for those half-open connections and spikes in SYN traffic, they’re clear red flags of a SYN flood attack. 🔍
