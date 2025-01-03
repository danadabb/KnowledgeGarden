---
id: jeybygpftmwnk69ylywov78
title: Solutions Architect Associate
desc: "Notes for the SAA certification"
updated: 1735880973752
created: 1734484581601
---

Notes attributed to [this course](https://learn.cantrill.io/courses/enrolled/1820301)

## AWS Accounts

- AWS account is a container for identities (users) and resources
- Every AWS account has a root users
- The account root user can't be restricted it has full access to everything within this account.
- The credit card used with the root account will be the Account Payment method, everything will be billed to that card
- AWS is a pay-as-you-go/consume platform
- Certain resources have a free-tier
- IAM - every AWS account comes with it's own IAM DB
- IAM lets you create 3 different IAM profiles - Users, groups and roles
  - Users represent humans or applications that need access to your account - this is for individual purposes
  - Groups are a collection of related users
  - Roles can be used for granting external access to your account
- IAM policy - used to allow/deny access to AWS services attached to other identities
- IAM is an identity provider, which also authenticates and authorizes
- IAM Access Keys are long term credentials with up to 2 available per IAM user typically used in CLIs or applications
- Access Keys are made up of Access Key ID and the Secret Access Key

## Technical fundamentals

Pre-cursor to the concepts covered in this course summarized here:
[[architecture.tech-fundamentals]]

## AWS Fundamentals

### Public vs Private services

- Can be categorized into two types: public and private services
- AWS public and private service are separated by network access.
- Public service is something which can be accessed using public endpoints e.g. s3
- A private aws service runs within a vpc so only what is connected to that vpc can access it
- AWS has three zones - the public internet zone, the private network and the AWS public zone which runs in between the public and private zone.
- AWS public zone is where public services operate from e.g. s3

### AWS Global Infrastructure

- AWS have created their infrastructure platform consisting of isolated regions connected together.
- A region is a creation of AWS which covers an area over the world which contains a full deployment of AWS infrastructure. New regions are added all the time.
- When interacting with most AWS services you're doing it at a particular region e.g. elastic compute cloud in North virginia is different to interacting to elastic compute in Sydney.
- AWS also provides Edge locations which are smaller than regions and they typically have only content distribution services as well as some types of edge computing. They are useful for companies like Netflix who want to store tv shows and movies as close to their customers as possible to allow low for low latency and high speed distribution
- Some services act from a global perspective e.g. IAM
- Regions have three main benefits:

1. Each reason is separate geographically which isolates any faults
2. Geopolitical separation - difference governance depending the region
3. Location control - tune architecture performance relative to an area

- Regions also have a code e.g Sydney is ep-southeast-2 and a name Asia Pacific (Sydney)

Inside every region, AWS also provide multiple availability zones. These give isolated infrastructure within a region. If a region experiences an isolated issue but only one availability zone is affected, the others are likely to be still fully functional. A solutions architect may distribute the services across multiple availability zones. This is used to build resilience.

### AWS Default Virtual Private Cloud (VPC)

- A VPC is a Virtual Network inside AWS
- A VPC is a regional service that operates within that region
- A VPC by default is private and isolated. Services deployed into the same vpc can communicate but it's isolated from other vpcs and the AWS zone/public internet.
- There are two types of VPCs per account:

1. Default VPCs - can only have one per region. Configured by AWS.
2. Custom VPCs - can have many per region. You use these in almost all serious AWS deployments to configure them how you like.

- VPCs cannot communicate with each other without you configuring them to do so.
- VPCs are regionally resilient
- The default VPC gets a default CIDR IP range which is always the same - 172.31.0.0/16
- A VPC can be subdivided into subnets for resilience. Each subnet inside a VPC can be put into an availability zone. The default VPC has one subnet in every availability zone in that region.
- the default VPC assigns a public address to the services by default.

### Elastic Compute Cloud (EC2)

- EC2 is a service which allows you to provision virtual machines known as instances with an operating system.
- EC2 is IAAS (Infrastructure as a Service) which provides access to virtual machines (instances)
- An instance is just an operating system configured in a certain way
- EC2 is a private AWS service by default - it uses VPC. You can configure it to have public access
- An EC2 is AZ (availability zone) resilient. If the AZ fails then the instance fails
- You can choose an instance with various sizes and capabilities
- EC2 provides on-demand billing - per second
- Instances can use different types of storage e.g. local host storage (ec2 host) or Elastic Block Store (EBS) which is network storage made available
- EC2 instances have a state e.g. RUNNING -> STOPPED -> TERMINATED
- EC2 can be moved from RUNNING TO STOPPED and back again
- TERMINATING an instance is a one way change, you can do that from the RUNNING or STOPPED state. It's a non-reversible action
- At a high level, an instance is composed of CPU, memory, disk and networking. You are charged for all four of those instances.
- When an instance is STOPPED, it means no CPU, memory or network is being used therefore you won't be charged for any running costs of that instance. Storage however is still being used when it's in the stopped state which means you will be charged for it.
- In order to have no costs for an EC2 instance you need to terminate it.
- An Amazon Machine Image (AMI) is an image of an EC2 instance.
- An AMI can be used to create an EC2 instance or an AMI can be created from an EC2 instance.
- An AMI is similar to a server image in a physical server
- An AMI contains:
  - Attached permissions - who can use the image e.g only owner vs specific accounts vs public
  - Root volume - the drive that boots the operating system
  - Block device mapping - configuration which links the volumes that the AMI has and how they're presented to the operating system e.g boot vs data volume
- EC2 can host different OS e.g. linux, windows, macos. You can connect to them via remote desktop (windows) or SSH (linux/macos)

### Simple Storage Service (S3)

- S3 is a global storage platform - it's regionally resilient. The data is replicated across availability zones in that regions. It can tolerate a fault in an AZ
- S3 is a public service
- It's used to host a large amount of data e.g. movies, audio, photos, text, large data sets
- Economical and can be accessed via a variety of methods e.g. UI/CLI/API/HTTP
- S3 delivers two main things:

1. Objects - the data s3 stores
2. Buckets - containers for objects

**Objects** are basically files that are made up of two components:

1. the object key (name) - usually the file name
2. the object value (data) - the data or contents of the object

- The bucket name needs to be **globally unique** - this is across all regions and aws accounts. It should be between 3-63 characters, all lower case, no underscores. Must start with a lowercase letter or a number. It can't be formatted like an IP address.

- A bucket can hold an unlimited number of objects and an unlimited amount of data - it's an infinitely scalable service.

- A bucket may show on the UI that it has folders but the underlying structure is flat and everything sits in the root. Folders are referred to as prefixes in s3 as they prefix the object names.

- There is a soft limit of 100 buckets for an s3 account and a hard limit of 1000 buckets using support request.

- You can have unlimited objects in a bucket, with each object able to range between 0 to 5TB in size.

- S3 is not a file or block. It is an object store
- You can't mount an s3 bucket as a drive e.g. K:\ or /images
- s3 is great for large scale data storage, distribution or upload
- great for offloading - moving data from a server to the s3
- Most services can use s3 as an INPUT or OUTPUT. s3 is a good default for data storage.

### CloudFormation (CFN) basics

- CloudFormation is Infrastructure as Code (IaC) which allows automation infrastructure creation, update and deletion.
- CFN uses templates written in either YAML or JSON
- A template:
  - has a list of resources to do the action on (at least one - mandatory)
  - description - the only restriction with this is if the template has an AWSTemplateFormatVersion, the description must come directly after it (this can be a trick question in the exam)
  - metadata - controls how the UI presents the template
  - parameters - adds fields which need to be added with input (default values could be provided)
  - mappings - allows you to create lookup tables
  - conditions - decision making in the template
  - outputs - once the template is finished it can present outputs based on the resource e.g. the instance ID of the ec2
- CloudFormation takes a template and creates a stack. A stack contains all the logical resources the template tells it to contain. CFN will create a corresponding physical resource in your AWS account.
- You can update or delete the logical resources in the template and the template will do this to the physical resources on your account
- CFN exists to automate infrastructure
- CFN can be used as part of change management as it can be put in code repositories
