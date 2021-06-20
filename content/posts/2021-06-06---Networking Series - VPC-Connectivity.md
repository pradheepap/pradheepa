---
title: VPC Connection - Same Region
date: "2021-06-20T00:00:00.121Z"
template: "post"
draft: false
slug: "networking-vpc-connnectivity"
category: "AWS Networking"
tags:
  - "Networking"
  - "Primer"
  - "AWS"
  - "VPC Connectivity"
  - "VPC Peering"
description: "If VPC a isolated virtual network, is there a provision to connect two different virtual networks."
socialImage: "/media/vpc-connectivity.jpg"
---
### Introduction
Often times, one logical group of virtual network (VPC) would want to interact with the other. AWS has provided options to connect between VPCs of the same region and also different regions.

### VPC Peering
VPC Peering is a networking connection provided by AWS to connect two VPCs. The VPCs can be from the same or different accounts. Since VPC is a region based entity, VPC Peering can also be established between VPCs from different regions in the same account. Once the peering is established, the instances in either of the VPCs can connect with each other as if they are in the same account.

  ![VPC Peering](/media/aws-vpc-peering.png)

### VPC Peering - Same Region
  To create a Peering Connection, go to VPC dashboard and click on `Create Peering Connection`. The wizard looks like as below. We need to provide the local VPC which we are interested to peer with and its CIDR block. If we are connecting to the same region in the same account, then we can select `My Account` and `This region` and provide the VPC and its CIDR to be connected to.

  ![Create VPC Peering](/media/peering-connection-wizard.png)

  After creating, the peering connection looks like below, and shows the peering connection status as `Active`.

  ![VPC Peering Config](/media/peering-connection-config.png)

  The next step after creating the VPC peering connection is to update the route table of the subnets in both the VPCs. 

  ![VPC Peering Route Table](/media/vpc-peering-subnet-route-table.png)

  VPC peering is a one-to-one connection between VPCs. So, to connect to another VPC, another peering connection has to be created and the route tables to be updated. 

  ![Multiple VPC Peering Multiple](/media/vpc-peering-multiple.png)

  If VPC A has a connection with VPC B and VPC C, this does not mean that a peering connection is automatically available between VPC B and VPC C since B is connected to A. Another peering connection has to be created to establish this relationship.

  ![Multiple VPC Peering No Connection](/media/vpc-peering-invalid-route.png)
  
### Things to know

- VPC Peering allows to reference security groups from the peer VPC in the same region.
- Supports DNS hostname resolution to return private IP address.
- Supports peering of both IPv4 and IPv6 addresses.
- The VPCs cannot have overlapping IP addresses.
- No redundant Peering connection can be made between the same VPCs. Only one peering connection is supported.

### Can you answer the following questions?

 1. Can NACL be accessed among VPCs?
 2. How to health check the VPC Peering Connection?
 
 
### References
1. https://d1.awsstatic.com/events/reinvent/2019/REPEAT_2_AWS_networking_fundamentals_NET201-R2.pdf