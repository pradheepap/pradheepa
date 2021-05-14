---
title: "Serverless Framework - What should you know?"
date: "2021-05-14T00:00:00.000Z"
template: "post"
draft: false
slug: "serverless-framework-an-intro"
category: "Serverless Framework"
tags:
  - "Lambda"
  - "Nodejs"
  - "Serverless Framework"
description: "Serverless Framework helps to get you started in building serverless applications in no time. Lets get started with the basics."
socialImage: "/media/image-0.jpg"
---

**zero-friction serverless development - www.serverless.com.** Having looked at an overview of what a serverless architecture in our [previous post](/posts/serverless-an-introduction), the next step would be to build one. And having to learn to many things before actually building a serveless application would cause friction. Hence I chose the Serverless Framework which requires you to know about yml/json. Hence I picked this for my serverless journey. 


## Getting Started

+ Install Serverless Framework.
+ Create a project based on serverless template.
+ Verify the resources created in CloudFormation Stack.

### Install Serverless Framework
  
```
npm install -g serverless
```

### Create a serverless project with node js template
Let us use the Nodejs template of serverless framework to create the new project. 

```
sls create --template aws-nodejs --path serverless-demo
```

#### serverless.yml

The following simple snippet deployed an AWS Lambda to your account

```
service: serverless-demo
frameworkVersion: '2'

provider:
  name: aws
  runtime: nodejs12.x
  lambdaHashingVersion: 20201221


functions:
  hello:
    handler: handler.hello
```

#### Deploy

Assuming AWS_PROFILE is set in the path in the terminal, let us go ahead and deploy the project created without making any changes.

```
serverless deploy
```

#### Logs
It will package and deploy the resources in the AWS Account as shown in the logs below.

```
Serverless: Packaging service…
Serverless: Excluding development dependencies…
Serverless: Creating Stack…
Serverless: Checking Stack create progress…
……..
Serverless: Stack create finished…
Serverless: Uploading CloudFormation file to S3…
Serverless: Uploading artifacts…
Serverless: Uploading service serverless-demo.zip file to S3 (390 B)…
Serverless: Validating template…
Serverless: Updating Stack…
Serverless: Checking Stack update progress…
……………
Serverless: Stack update finished…
Service Information
service: serverless-demo
stage: dev
region: us-east-1
stack: serverless-demo-dev
resources: 6
api keys:
 None
endpoints:
 None
functions:
 hello: serverless-demo-dev-hello
layers:
 None
```

The log summary shows that, it had created,
+ default environment as dev (stage) variable.
+ default region : us-east-1
+ Cloudformation stack is named serverless-demo-dev (service name + stage name defined in serverless.yml)
+ 6 AWS resources. We will look into the CloudFormation stack to see what were all the AWS resources created.
+ Created a lambda function named serverless-demo-dev-hello. ( service name defined in serverless.yml + function name defined in serverless.yml + stage in serverless.yml ). This is the default name generated for the lambda function but can be overridden.

### CloudFormation Stack

While looking into the CloudFormation stack, following are the AWS resources created.
AWS::Lambda::Function — serverless-demo-dev-hello
AWS::Lambda::Version — arn:aws:lambda:us-east-1:AccountID:function:serverless-demo-dev-hello:1
AWS::Logs::LogGroup — /aws/lambda/serverless-demo-dev-hello
AWS::IAM::Role — IamRoleLambdaExecution
AWS::S3::Bucket — S3 Bucket for our package
AWS::S3::BucketPolicy — S3 Bucket Policy
Any serverless projects deployed would create an S3 Bucket and a Bucket Policy attached to the bucket that has the cloudformation template and the package deployed containing the code for the lambda functions.
In this serverless project, a lambda function and an IAM role attached to the lambda function to CreateLogStream, CreateLogGroup and PutLogEvents’, a log group where the lambda’s log events will be traced were created.


#### Serverless High Level Architecture Overview
The high-level architecture would look like as below,
![Serverless Framework](/media/serverless-framework-nodejs.png)