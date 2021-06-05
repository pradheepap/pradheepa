---
title: CIDR Block
date: "2021-06-05T00:00:00.121Z"
template: "post"
draft: false
slug: "networking-cidr"
category: "AWS Networking"
tags:
  - "Networking"
  - "Primer"
  - "AWS"
description: "Ever wondered what is a CIDR block while creating VPC and how to allocate IP addresses to the subnets."
socialImage: "/media/networking-series-cidr.png"
---
### Introduction

Before working in serverless technologies, most of the applications I worked on were hosted on-prem. I used to interact with the infrastructure team and the questions they ask were sometimes not understandable to the application developers. The talk about firewall, ip addresses, ssl, ftp, connect direct connections are too much to comprehend initially. With serverless applications development though the application developer need not have to know about these terminologies, its good to understand what's behind the scenes. 

![Networking Series](/media/networking-series-cidr.png)

AWS opens the doors for the application developers to know about the networking configuration used by their application. Usually in an on-prem applications the networking services are managed by a different team and the developers are not aware of the configuration. The interaction typically happens where the network team share the snippets of the logs whenever an error occurred connecting to our application. But with more applications being developed on cloud services, developers can atleast access the lower environment configurations (dev or staging) which is usually mimicked to production. Let us start the series by getting to know what is a CIDR Block

### CIDR Block (Classless Inter-Domain Routing)

![CIDR](/media/vpc-creation-cidr.png)

While creating VPC in AWS, the second step (Refer image above) is to provide the IP CIDR block i.e the range of IP addresses to be allocated to this VPC. It is of the format 10.0.0.0/16 (IP address followed by / and a number). Most of us knew the IP address format but what does this number signify in CIDR Block. 

- CIDR is an industry standard.
- The number is the number of bits in an IP address that must match to be considered as part of the selected CIDR block.
- Each IP Segment i.e the number between the dots is of eight bits that makes the entire segment consisting of 32 bits.
- Let us take the ip address `10.0.0.0` and represent them in binary as `0000 1010. 0000 0000. 0000 0000. 0000 0000`. 
- Let us take the CIDR block notation as `10.0.0.0/16`.  The number here mentions that any ip address that has the first 16 digits matched is part of this CIDR IP block. Since the first 16 bits has to remain unchanged, it leaves room for the rest of the 16 digits to take any value. Hence 2<sup>16</sup> `65536` ip addresses are available in this range.

### Additional Resources
https://www.colocationamerica.com/ip-calculator
