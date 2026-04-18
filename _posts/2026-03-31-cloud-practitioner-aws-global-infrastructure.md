---
title: Cloud Practitioner - AWS Global Infrastructure
image:
  path: /assets/2026-03-31-cloud-practitioner-aws-global-infrastructure/1_WByN2mVIjFNWzdnfv7kdMQ.webp
date: 2026-03-31 06:00:00 -0600
categories: [Cloud Computing, AWS]
tags: [cloud-practitioner, AWS]     # TAG names should always be lowercase
---

The **AWS Global Infrastructure** consists of hardware and data centers distributed and interconnected all around the world to act as one large resource for the end customer.  

The AWS global infrastructure is composed of: 

* Regions.
* Availability Zones (AZs). 
* Points of Presence (PoP).
* Direct Connect Locations. 
* Local Zones. 
* Wavelength Zones.

## AWS Regions

An **AWS Region** is a physical, geographical area that hosts multiple data centers. An AWS Region is isolated from other AWS Regions, but they're interconnected through high speed fiber network. 

When choosing an AWS Region to deploy your workloads consider this key factor first:

* **Regulatory Compliance**: The choice of your AWS Region might be affected by you compliance requirements. If your company must comply with regulations that require your data or costumer data to be stored in a specific geographic territory, then choose the AWS Region that meets this compliance requirement: Your data must live within the boundaries of the UK? then you should choose the AWS London region. 

Since not all companies are bound by regulatory controls that dictate their potential AWS Region, consider focusing on these 3 other factors: 

* **Latency**: How close you're to your customer base?. If most of your customers live in Singapur then consider choosing the Singapur AWS Region.
* **Feature Availability**: Sometimes the region you choose might not offer the AWS features you need, keep this factor in mind. 
* **Pricing**: Prices vary from AWS Region to AWS Region. Running the same workload can either be cheaper or more expensive depending on AWS the region. Therefore, budget must be your primary concern.

### The us-east-1 (Northern Virginia) Region

The largest and the most important region is **us-east-1 (Northern Virginia)**. This AWS Region is the most up-to-date region of all, and it is where your billing information resides.

New services are most likely to debut in this region. 

## Availability Zones

An **Availability Zone** is a physical location within an AWS Region that hosts a single or a group of data centers (clusters of data centers):

![Desktop View](/assets/2026-03-31-cloud-practitioner-aws-global-infrastructure/Pasted_image_202407201445391.png)

Generally speaking, each AWS Region has at least 3 Availability Zones, and each AZ is located tens of miles apart from other AZs within the same AWS Region.

## Points of Presence (PoP)

 A **Point of Presence (PoP)** is intermediate location between an AWS Region and the end user. It's basically *a data center owned by AWS or a trusted partner for content delivery*. Either way, a PoP is used by AWS Services to *accelerate communication* and *get content close* to your customers no matter where they are in the world.

### Edge Locations

From an AWS perspective, a PoP is nothing but an **Edge Location**.

An **Edge Location** is a group of data centers that hold a copy of your *origin* files. Their purpose is to get your content (web pages, images, video, etc) closer to where your end users are physically located.

An **Edge Location** is a critical component of CDNs (Content Delivery Networks), for Instance, CDN services like *AWS CloudFront* rely heavily on Edge Locations. Other services such as *DNS Route53*, *VPC Endpoints*, *Amazon S3 Transfer Acceleration*, and *AWS Global Accelerator* rely on Edge Locations too, to reduce latency.

## The AWS Global Network

Also know as the **AWS Backbone**. Basically, it's a private, global network that interconnects AWS Regions, AZs, and Edge Locations, allowing data to travel quickly and securely **_without the need to traverse the public Internet_**.

### AWS Public & Private Services

In the context of the AWS Backbone, an AWS Service can be labeled as **Public** or **Private** depending on how it interacts with the AWS Network Infrastructure.

### AWS Public Service

A **public service** is a service that can be reached from anywhere there is an Internet connection. Looking at this diagram, you can appreciate an **AWS Public Zone**:

![Desktop View](/assets/2026-03-31-cloud-practitioner-aws-global-infrastructure/Pasted_image_202409121732181.png)

An **AWS Public Zone** lives inside the AWS Global Network, and it can communicate openly with the Internet. Basically, any AWS Service is considered *public* when it sits and runs from this public zone. 

Public services often expose endpoints to the Internet for end-user interaction, but keep using the AWS Infrastructure to optimize delivery and security. For example:

* **AWS S3 and CloudFront**: These services are accessed from the public Internet and use edge locations to serve content globally. However, the traffic is optimized by routing it through the AWS Backbone as much as possible. 
* **API Gateway**: Offers public-facing APIs, but requests coming from the Internet are processed within AWS Regions while optimizing data traffic through the AWS Backbone. 
* **Elastic Load Balancer (EBS)**: Internet-facing load balancers take and distribute traffic coming from the Internet to backend resources inside AWS, but using the AWS Backbone internally. 

Just remember, a public zone is not the public Internet, but it connects to it.

### AWS Private Service

A **private service** is a service that remains entirely within the AWS Backbone, routing data within AWS Internal Network and **_avoiding the public Internet entirely_**.  

A **private service** basically lives inside an **AWS Private Zone**. Looking at the same diagram, you can appreciate this private zone neither have direct connection to the open Internet nor to an AWS Public Zone:

![Desktop View](/assets/2026-03-31-cloud-practitioner-aws-global-infrastructure/Pasted_image_202409121755401.png)

Inside a private zone you create isolated environments such as a VPC or an EC2 Instance, and by default, they will be completely isolated. Nonetheless, you can add permissions to your VPC or EC2 Instances to access the public zone. 

By default, the only services that can access services inside the private zone, are services inside the private zone. Also by default, keep in mind that there are no permissions for the private zone, except for your local routes. 

Examples of private services:

* **AWS VPC**: Resources within a VPC communicate using the AWS Backbone, allowing private communication between EC2 Instances, RDS Instances, etc. 
* **VPC Peering**: Two VPCs can communicate to each other across AWS Regions using the AWS Backbone. 
* **AWS PrivateLink**: Enables private communication to AWS Services or third-party Services over the AWS Backbone without exposing traffic to the public Internet. 
* **Direct Connect**: Providers a dedicated physical connection from your on-premises data centers to AWS over a private network, bypassing the public Internet.

## Scopes of AWS Services

Each service is different, you have to understand that the scope of a service might affect your application architecture. 

### Global Resilient Services

It's a service that operates across multiple AWS Regions. _If an AWS Region fails, the service continues to run_. 

Examples of Global services: 

* AWS IAM. 
* Route53. 
* CloudFront. 

The AWS Region for these type of services is commonly label as _"Global"_. 

### Regional Resilient Services

It's a service that operates within an specific AWS Region. Usually replicates the data into multiple _Availability Zones_ inside that AWS Region. _If you lose one AZ, the service continues to run_. 

Examples of Regional services: 

* AWS EFS. 
* AWS ELB. 
* EC2 Auto Scaling Group. 
* AWS Batch. 

If you're not asked to specify an individual Availability Zone to deploy a service, it's an indicator that the services operates on a Region-scope level. 

### Availability Zone Resilient Services

It's a service that operates in a single _Availability Zone_. Usually replicates the data into multiple _data centers_ inside that AZ. _If you lose the AZ, the service will fail_. 

Examples of Availability Zone services: 

* AWS EBS. 
* AWS EC2. 

While Global and a Regional services require no additional effort on your part to achieve High Availability, with Availability Zone services you're responsible for increasing the availability and durability of your resources.

## Direct Connect Locations (DCLs)

A _direct connect location is a trusted data center partner_. Through a DCL you can establish a dedicated high-speed, low-latency connection from our on-premises infrastructure to AWS. 

### AWS Direct Connect

It's the AWS offering that makes possible the _physical, dedicated, private connection_ between your on-premises infrastructure and AWS. 

This service has two network connection options: 

* **Lower Bandwidth**: 50Mbps to 500Mbps. 
* **Higher Bandwidth**: 1Gbps to 10Gbps.

## Local Zones

A **local zone** is a group of data centers located very close to high-density population areas to provide single-digit millisecond latency for that particular area. It is designed to enhance the performance for highly-demanding applications that are sensitive to latency, for example: Media & Entertainment, Ad-Tech, Machine Learning, etc. 

_A local zone is a logical extension of an AWS Region_, for example, the local zone for Los Angeles California `us-west-2-lax-1a` is tied to the US-West Region.

Bear in mind that not all AWS services are available in these local zones.  

## Wavelength Zones

Edge Computing seeks to bring processing power closer to the locations where data originates, as a result, latency is dramatically reduced and data processing speed is enhanced, all of this while getting all the well-known cloud's benefits: security, scalability and high availability. 

Edge computing is particularly useful for real-time applications like IoT devices and autonomous vehicles where quick data processing is paramount. 

Through **AWS Wavelength Zones** you can extend AWS infrastructure to the edge of telecommunication networks by leveraging edge computing.  

You see, AWS has partnered with various Telecom companies such as Verizon, Vodafone, KDDI, and so on, to utilize their 5g networks. This way you can create a Subnet tied to a Wavelength Zone and from there start deploying EC2 Instances. It would be as though you were running virtual machines over 5g networks.  
