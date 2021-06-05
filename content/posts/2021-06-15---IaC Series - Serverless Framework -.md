---
title: Arrays
date: "2021-06-06T00:00:00.121Z"
template: "post"
draft: true
slug: "ds-array"
category: "AWS Networking"
tags:
  - "DataStructures"
  - "Java"
  - "Basics"
  - "Arrays"
description: "To deploy any instances in AWS, it has to be part of VPC. But what is VPC?"
socialImage: "/media/networking-series-cidr.png"
---
### Introduction
 VPC is termed as Virtual Private Cloud.

![Networking Series](/media/networking-series-cidr.png)


### Default VPC

![CIDR](/media/vpc-creation-cidr.png)

While creating VPC in AWS, the second step (Refer image above) is to provide the IP CIDR block i.e the range of IP addresses to be allocated to this VPC. It is of the format 10.0.0.0/16 (IP address followed by / and a number). Most of us knew the IP address format but what does this number signify in CIDR Block. 

- CIDR is an industry standard.
- The number is the number of bits in an IP address that must match to be considered as part of the selected CIDR block.
- Each IP Segment i.e the number between the dots is of eight bits that makes the entire segment consisting of 32 bits.
- Let us take the ip address `10.0.0.0` and represent them in binary as `0000 1010. 0000 0000. 0000 0000. 0000 0000`. 
- Let us take the CIDR block notation as `10.0.0.0/16`.  The number here mentions that any ip address that has the first 16 digits matched is part of this cidr IP block. Since the first 16 bits has to remain unchanged, it leaves room for the rest of the 16 digits to take any value. Hence 2<sup>16</sup> `65536` ip addresses are available in this range.

### Additional Resources
https://www.colocationamerica.com/ip-calculator
