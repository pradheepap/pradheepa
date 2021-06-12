---
title: Virtual Private Cloud (VPC)
date: "2021-06-06T00:00:00.121Z"
template: "post"
draft: false
slug: "networking-vpc"
category: "AWS Networking"
tags:
  - "Networking"
  - "Primer"
  - "AWS"
  - "VPC"
description: "To deploy any instances in AWS, it has to be part of VPC. But what is VPC?"
socialImage: "/media/aws-vpc.png"
---
### Introduction
 Amazon VPC (Amazon Virtual Private Cloud) is used to launch AWS resources into a virtual network. The various components of the Amazon VPC defines the traffic, security, routing configurations etc. 

### Regions and Availability Zones
  AWS Global Infrastructure at the high level were distributed as Regions. Region is a physical location in the world like Ireland, London, Singapore etc. Each region is further composed of Availability Zones (AZ). AZ are nothing but discrete data centers. For example, if Singapore is the region, then within that region, then AWS has 3 different data centers located in the east, west, south of Singapore, that are isolated from one another. So if there is a power outage/ accidental fire in one data center, the other data center will continue to operate without causing any disruption.

### Amazon Virtual Private Cloud
  Having defined the regions and availability zones, the following diagram helps to fit VPC in the global infrastructure. Amazon VPC is the virtual network that span across multiple Availability Zones in a Region. Lets say am going to launch an EC2 instance which is going to serve my static website. Following are the considerations I have to make to host my instance.

  ![Networking Series](/media/amazon-vpc-overview.png)

  
  1. `Pick the region`. Let's say I chose Singapore. Picking a region close to the users accessing the application is important. Otherwise there will be network latency.
  2. `Pick the Availability Zone`. We have three availability zones in this region. The AZs are usually named as 1a, 1b, 1c etc. Let us assume I pick 1a.

VPC is defined at the region level and it spans across multiple availability zones. So when I create an instance inside the VPC, I can choose to host them in any of the AZ. The instance is the physical server that has an IP address and can be placed inside a Availability Zone. What if something happened at that AZ. All the instances placed in the AZ may be possibly lost. To avoid such situations, we can host another instance in another AZ but inside the same VPC.

The IP address range in a availability zone is defined by the VPC CIDR block IP range. Lets consider the default VPC's IP range for the CIDR Block which is 172.31.0.0/16. If you refer to the article on CIDR Block IP range, the /16 means the first 16 bits to remain unchanged and hence the first two IP segments cannot take any other value. This provides us with the room to change the last two IP segments to any number between (0-255). 

For the ease of convenience, let us further divide the third IP segment to different values as 172.31.1.0, 172.31.2.0, 172.31.3.0. This could mean that the range of IP addresses were grouped as `(172.31.1.0 - 172.31.1.255)`, `(172.31.2.0 - 172.31.2.255)`, `(172.31.3.0 - 172.31.3.255)`. These three logical grouping would be called as `subnets`. Subnets are group of logical IP addresses in an AZ. We can have more than one subnets in the AZ. But why do we need to further group them? We can have a logical group open to public (Connected to internet) and the other private (it can only communicate within the instances inside the VPC but not to the internet) which is a very common usecase.

![Subnets](/media/subnets.png)

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
