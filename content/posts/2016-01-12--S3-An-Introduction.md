---
title: S3 - Simple Storage Service
date: "2016-12-01T22:40:32.169Z"
template: "post"
draft: false
slug: "s3-an-intro"
category: "S3"
tags:
  - "Serverless"
  - "S3"
  - "Storage"
description: "S3 is an object storage service in AWS. I would say, the first part of getting to build a serverless architecture is to get to know the components adapted to this architecture. S3 is the first piece to be picked on"
socialImage: "/media/aws-s3.png"
---

**As an object storage service, S3 provides a place to store your data.** Since the servers are ephemeral, the data should not be. So a simple storage solution would be to store in S3. 

![Simple Storage Service](/media/aws-s3.png)
## Serverless Framework

+ Makes building of serverless services quick and easy.
+ Can be used for all the serverless providers.
+ We are going to use Serverless Framework to manage our Infrastructure as Code.


### Install Serverless

```
npm install -g serverless
```

### Create a serverless project with node js template

```
sls create --template aws-nodejs --path s3-static-website
```

 A boiler plate code is generated. Open the folder in your favourite editor(I am using VS Code). I am using AWS and I have setup my default profile

 The boiler plate generated has a file `serverless.yml`. 

Donec non enim in turpis pulvinar facilisis. Ut felis. Praesent dapibus, neque id cursus faucibus, tortor neque egestas augue, eu vulputate magna eros eu erat. Aliquam erat volutpat. Nam dui mi, tincidunt quis, accumsan porttitor, facilisis luctus, metus.