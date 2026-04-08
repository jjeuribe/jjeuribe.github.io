---
title: Cloud Practitioner - Getting to know AWS
image:
  path: /assets/2026-03-30-cloud-practitioner-getting-to-know-AWS/1_y1x7kOARssTx2OTjTNV33g.webp
date: 2026-03-30 09:00:00 -0600
categories: [Cloud Computing, AWS]
tags: [cloud-practitioner, AWS]     # TAG names should always be lowercase
---

## What is Cloud Computing?

It’s just simply the on-demand delivery of IT resources over the internet with a pay-as-you-go pricing.

## What is a Cloud Provider?

It’s just a company that offers a wide range of cloud-based services, from a few options to thousands. However, the key feature is that you can create large and complex architectures by chaining these services together.

Typically, the consumption of these cloud-based services is charged by the second, hour, etc.

### Cloud Provider vs On-Premises System

* **On-Premises**: you’re the owner of your IT resources, you must hire staff to configure and manage your physical IT infrastructure, you also need a physical place to house all these physical devices. *You assume all responsibility and the associated risks*.
* **Cloud Provider**: you don’t own any servers or hardware but you’re not responsible for their maintenance either. There’s no need to hire staff or own a physical location to manage and host the resources. However, *there is a shared responsibility*. You’re only responsible for the configured services that you use and the code you deploy, while the Cloud provider takes care of the underlying infrastructure.

### Landscape of Cloud Service Providers

The **Top-Tier**, well-known providers are those who went early to market. They offer a wide range of services with strong synergies between them: 

![Desktop View](/assets/2026-03-30-cloud-practitioner-getting-to-know-AWS/Pasted_image_20240918132618.png)

**Mid-Tier providers**, backed by well-known tech companies, tend to innovate slowly and shifted their focus towards specialization:

![Desktop View](/assets/2026-03-30-cloud-practitioner-getting-to-know-AWS/Pasted_image_20240918133140.png)

**Light-Tier** providers started as Virtual Private Server (VPS) Providers, but now they have transitioned to offer IaaS services:

![Desktop View](/assets/2026-03-30-cloud-practitioner-getting-to-know-AWS/Pasted_image_20240918133629.png)

They're very light in terms of their offerings. However, they are simple and cost-effective.

As you can witness in this Magic Quadrant, when it comes to Cloud Infrastructure and Platforms services, AWS is still the leader, only followed by Microsoft and Google:

![Desktop View](/assets/2026-03-30-cloud-practitioner-getting-to-know-AWS/Pasted_image_20240918132129.png)

## Type of Cloud Computing

### SaaS - Software as a Service

It's just a finished digital product, ready for end *user's consumption*. 

A SaaS product is run and managed by the provider. You as the end user, don't need to worry about how the services are ran, updated and maintained. 

Examples of SaaS products: Gmail, Google Docs, Asana, Hotmail, Google Drive, OneDrive, etc.

### PaaS - Platform as a Service

It's a *developer-oriented* digital product. You as a developer, don't need to worry about provisioning, configuring or understanding physical hardware and operating systems. Your only concern is to develop, deploy and manage your applications. 

Examples of PaaS products: Heroku, Linode, AWS Beanstalk, Google App Engine, etc. 

### IaaS - Infrastructure as a Service

This type of service is ideal for *IT professionals*. 

IaaS is the building block for cloud IT. It provides access to networking features, computers, and data storage. You don't need to worry about hiring IT staff, building out data centers, or acquiring expensive hardware. 

Examples of IaaS products: AWS, Microsoft Azure, Digital Ocean, Oracle Cloud, etc. 

These are the 3 fundamental types of cloud computing. However, in practice, there are many other *"x as a Service"* offerings out there, such as: 

* **DbaaS**: Database as a Service. 
* **FaaS**: Function as a Service. 
* **CaaS**: Communication as a Service.
* **MaaS**: Metal as a Service. 
* **NaaS**: Network as a Service. 
* **IDaaS**: Identity as a Service.
* etc. 

However, even though you don't have to memorize all of them, it's important to have a solid grasp of the different Types of Cloud Computing. This will help you to understand and separate the responsibility you have as the user of the service in question, and the responsibility the provider has with the service they are offering.

## Deployment Models for Cloud Computing

### Cloud-based Deployment

Also known as *public cloud*, *cloud-native* or *cloud first*. Basically, all the company's resources are deployed in the Cloud. 

This model is ideal for Startups, and new projects and companies. 

### On-premise Deployment

Also known as *private cloud*. Basically, all your IT resources are physically deployed in you company using virtualization and resources management tools such as OpenStack or Apache CloudStack. 

This type of deployment is most often used in the public sector: Government, Hospitals, large companies with *extensive and restrictive regulations* such as Insurance companies. 

### Hybrid Deployment

Basically, cloud-based resources are connected to on-premises infrastructure. This type of deployment is perfect for when you have applications that are better maintained on-premise, but you want to the get the most out of certain services in the cloud. 

For example, high-regulated workloads may remain on-premise, whereas, non-regulated workloads can be moved to the Cloud. 

Ideal for: Banks, Fintechs, Legacy Systems. 

### Cross-Cloud Deployment

Also known as *multi-cloud*. In this type of deployment you use *multiple Cloud Providers*.

## Benefits of Cloud Computing according to AWS

1. **Trade upfront expense for variable expense**: an upfront expense involves paying in advance for a resource you need before using it, in contrast, a variable expense means you only pay for what you consume. The benefit of choosing a variable expense over upfront expense is cost-saving, as it eliminates the need for heavy initial investment.  
2. **Stop guessing capacity**: sometimes you just don't know how much infrastructure capacity you'll need to deploy your app, stop guessing and paying for underutilized resources, scale up and down to meet the current demand efficiently.  
3. **Stop spending money to run and maintain servers**: instead focus on your apps and customers. 
4. **Increase speed and agility**: deploy workloads with just a few clicks instead of waiting days or weeks for your IT department to get the on-premise solution ready. 
5. **Benefit from massive economies of scale**: the aggregated cloud usage from a large number of customers results in lower pay-as-you-go prices. 
6. **Go global in minutes**: with just a few clicks deploy your application in multiple regions around the world.
