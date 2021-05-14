---
title: Serverless - An Introduction
date: "2021-05-13T00:00:00.121Z"
template: "post"
draft: false
slug: "serverless-an-introduction"
category: "Serverless Architecture"
tags:
  - "Serverless"
  - "Architecture"
  - "Web Application"
description: "Serverless does not mean no servers. Obviously we need some servers to host our content and process our events. Serverless Architecture takes off the load to manage the servers. It will spin-up the server on-need basis. Wondering how it will work? Let's see how."
socialImage: "/media/serverelss-app-arch.png"
---

Serverless does not mean no servers. Obviously we need some servers to host our content and process our events. Serverless Architecture takes off the load to manage the servers. It will spin-up the server on-need basis. Wondering how it will work? Let's see how.

![Serverless Architecture](/media/serverelss-app-arch.png)

The diagram shown above is an example of serverless architecture designed web application in AWS. In a conventional architecture, we have to provision our servers, plan the memory and CPU utilization for our expected workload and will deploy the code.

But in serverless architecture, all we need is the code to execute to be deployed. Let's say in an e-commerce application, an order is placed, an event is triggered and the serverless code listening to the order placed event will be triggered. So in serverless architecture, the management of servers is not in the scope of the developers. This lifts the huge weight from the developers shoulders because most of the time they might be wondering if the issue is happening due to an infrastructure problem or inside the application.

The architecture can be designed in such a way to keep up with the load with no specific limitations. With less cost and no servers to maintain, Serverless Architectures can solve most of our day-to-day requirements right from running a e-commerce website to static websites.