---
title: Virtual Private Cloud (VPC) - 101
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
 Amazon VPC (Amazon Virtual Private Cloud) is used to launch AWS resources into a virtual network. The various components inside the Amazon VPC defines the traffic, security, routing configurations etc. 

### Regions and Availability Zones
  AWS Global Infrastructure at the high level were distributed as Regions. Region is a physical location in the world like Ireland, London, Singapore etc. Each region is further composed of Availability Zones (AZ). Availability Zones (AZ) are nothing but discrete data centers. For example, if Singapore is the region, then within that region, then AWS might have 3 different data centers located in the East, West, South of Singapore, that are isolated from one another. So if there is a power outage or accidental fire in one data center, the other data center will continue to operate without causing any disruption.

### Amazon Virtual Private Cloud
  Having defined the regions and availability zones, the following diagram helps to fit VPC in the global infrastructure. Amazon VPC is the virtual network that span across multiple Availability Zones in a Region. 

  ![Networking Series](/media/amazon-vpc-overview.png)

  Amazon VPC is defined at the region level and it spans across multiple availability zones. So when instance has to be created inside the VPC, it can be chosen to host them in any of the availability zones inside the region. The instance is the physical server that has an IP address and are physically placed inside a data center (Availability Zone). 
  
  To visualize it, imagine placing your laptop (instance) in one of your office premises at location A(Availability Zone-a). You are visiting another branch at location B(Availability Zone - b) of your office on the next day and you have to resume your work. The instance you placed at location A cannot be physically accessible to you. But, if the data inside your laptop (instance) at location A can be copied to another laptop at location B, then you would be able to resume your work. You usually don't care about the physical location as long as we were able to perform the same function as you did with the laptop (instance) at A.

  The availability zones though might seem redundant, often most real world applications are hosted in more than one availability zones for the resiliency during the time of disruption. So when we define our virtual network (VPC), it has the option to span across multiple availability zones within the same region.
  
### CIDR Block
  
  To create a VPC, we need to define the [CIDR Block IP](/posts/networking-cidr) range. This defines the number of instances that a VPC can have. This defines how many IP addresses (instances) that can be placed inside this VPC. Again since this is at the VPC level, the IP addresses can span across multiple AZ. 
  
  Let us consider the default VPC's `IP CIDR Block` which is `172.31.0.0/16`. The /16 means the first 16 bits (172.31 segment) had to remain unchanged and hence the first two IP segments cannot take any other value. This provides us with the room to change the last two IP segments to any number between (0-255). So the default VPC can take 65536 IP addresses. 

  ![CIDR](/media/vpc-creation-cidr.png)

  ### Subnets

  ![Subnets](/media/subnets.png)

  We can further segment the IP address (Subnets) at the AZ level.  Let us consider the CIDR values for the two AZs `172.31.0.0/24` and `172.31.1.0/24`. This could mean that the range of IP addresses were grouped as `(172.31.0.0 - 172.31.0.255)`, `(172.31.1.0 - 172.31.1.255)` each with 256 IP addresses.  Now I am assigning these two IP ranges to two different Availability Zones. The first AZ will take the range as `(172.31.0.0 - 172.31.0.255)` and the second as `(172.31.1.0 - 172.31.1.255)`. Let us consider only one availability zone, because the other is going to be an exact replica. 
  
  I can use the entire 256 range as one logical group or can further divide them into multiple groups. These logical groups are called subnets (sub-networks). For illustration, let us consider two subnets within an AZ, then the CIDR range of the subnets can be `172.31.0.0/24` and `172.31.128.0/24` as 128 IP addresses each. 

  ![Subnets](/media/subnet-ip-range.png)


  ### Security Groups

  The  picture below shows that in an availability zone, we have created two subnets (private and public). As the name suggests, one subnet will allow internet access and the other will not. The traffic configuration has to be defined to make them public and private. 
  
  It a very common real-world scenario for hosting a web application. The instances acting as webservers are public and should be reachable via HTTP (Port 80) and HTTPS (Port 443) traffic, but the database instances or application servers will not be accessible to internet but only to webservers. As shown in the diagram below, we can define two security groups namely `Web Server Security Group` and `App Server Security Group`.

  ![Security Group Inside VPC](/media/aws-security-group.png)

  A security group is used to control inbound and outbound traffic. The `Security Groups` act at the instance level in a VPC. Following is the wizard for creating a security group. We need to define the allowed inbound traffic and outbound traffic and associate with the VPC which this security group needs to be part of. 

  ![Security Group Wizard](/media/security-group.png)

#### Summary
  - Act at the instance level.
  - Security group rules enable us to filter traffic based on protocols and port numbers.
  - The rules are stateful. For example if a port 443 is enabled for inbound, then 443 is enabled for outbound as well regardless of the outbound rules. 
  - There is no deny rule. Only allow.
  - Instances associated with a security group can't talk to each other unless the rules allowing the traffic has been added.

### NACL (Network Access Control List)
 It is similar to the security group which defines network access inside the VPC but this operates at the subnet level. If we were to define common traffic rule across the subnet, we define in NACL. Following is the configuration for the default NACL created inside the default VPC. Here we can see the subnet associations in the third tab. 

![NACL](/media/default-nacl.png)

This wizard says that these are all the allowed inbound and outbound rules and these are the subnets associated with this NACL rules. This is unlike the security group where we just define the inbound and outbound rules. We don't have to define the instances associated with the security group while creating it. The security groups are associated with the instances only in the instance creation wizard. But with NACL, it had to define the subnets associated with it. 
#### Summary
 - Operate at the subnet level.
 - Inbound and Outbound rules had to explicitly defined. For example, a port 80 allow inbound rule in NACL does not mean port 80 outbound rule like security group. If its not defined it will be automatically denied.
 - We can define the rules to both allow and deny.

 ### Route Tables
 Route tables is like a traffic controller to the subnet in the VPC and contains rules for which packets to go where. It is more like a directory of what are all the possible routes that can happen within the subnet. Often times, I confuse route table with NACL because both are associated with the subnet.

 A sample route table looks like below for a public subnet. If the VPC was set up to have the address space of `172.16.0.0/16`, the `local` route defined as `172.16.0.0/16` allows all of the resources created within the VPC to talk to each other without any additional configuration. The next configuration is the IPv6 config inside the VPC to talk to each other. Creating a config `0.0.0.0/0` and attaching the target to the Internet Gateway (This has be created ahead of configuring it in the route table) makes any subnet that is attached to this route table public because it has access to internet. 

  ![Sample Public Route Table](/media/route-table-eg.png)

  > To get to the internet, go via Internet Gateway

  ![Sample Private Route Table](/media/private-route-table.png)

  > To get to anything inside the VPC - stay local. No route anywhere else
 
 #### Summary
  - Each subnet can have only one route table. 
  - Route table can be shared across subnets.
  - All the routes for the subnet based on this route table. 
  - NACL and Security Groups are meant for securing the instances found under `SECURITY` section of the VPC create wizard and not meant primarily for routing. 
  - If we look at the diagram below, we can see the first point of contact for any traffic is the route table attached to the subnet and then NACL and security group.

 ![Route Table](/media/aws-route-table.png)
  
### Internet Gateway

 As the name suggests, this is the gateway to the public internet access. If we are hosting a web application, the web servers are usually public facing and be reachable via internet. In order to achieve this, we should first create an Internet Gateway and define the route in the route table. This allows internet traffic both to and from the instance.

![Internet Gateway](/media/internet-gateway.png)


### Network Address Translation (NAT) Gateway

 A NAT gateway is a Network Address Translation (NAT) service. The NAT gateway can be used so that the instances in a private subnet can connect to services outside the VPC but external services cannot initiate a connection with those instances. For example to patch security updates. There are two types of NAT Gateway, Public and Private. Public as the name says will be connected to the internet (one-way) and Private Gateway is used to connect other VPCs. A public NAT gateway is created in a public subnet and will be associated with an elastic IP address.

 ### Can you answer the following questions?

 1. Can a subnet exists without the route table?
 2. What is the difference between NACL and route table?
 3. What is a CIDR Block IP range? 
 4. If the route table is associated with the subnet, why it's shown in the VPC console as part of VPC configuration?
 5. How many route tables can a VPC have?
 6. What is a main route table in VPC and can you edit the main route table?
 7. Is it possible to create another route table and make that as a main route table?
 
### References
1. https://d1.awsstatic.com/events/reinvent/2019/REPEAT_2_AWS_networking_fundamentals_NET201-R2.pdf