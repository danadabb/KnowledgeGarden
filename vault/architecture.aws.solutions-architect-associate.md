---
id: jeybygpftmwnk69ylywov78
title: Solutions Architect Associate
desc: "Notes for the SAA certification"
updated: 1739419754751
created: 1734484581601
---

Notes attributed to [this course](https://learn.cantrill.io/courses/enrolled/1820301)

## AWS Accounts

- An AWS account is a container for identities (users) and resources
- Every AWS account has a root user
- The account root user can't be restricted it has full access to everything within this account.
- The credit card used with the root account will be the Account Payment method, everything will be billed to that card
- AWS is a pay-as-you-go/consume platform
- Certain resources have a free-tier
- IAM - every AWS account comes with it's own IAM Database
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

- Services can be categorized into two types: public and private services
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
2. Geopolitical separation - different governance depending the region
3. Location control - tune architecture performance relative to an area

- Regions also have a code e.g. Sydney is ep-southeast-2, as well as a name - Asia Pacific (Sydney)

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

### CloudWatch (CW) Basics

- CloudWatch is a support service which is used by many other AWS services. It collects and manages operational data detailing how it performs, runs or logging data
- It performs 3 main jobs:

1. **Metrics** - collects metrics from AWS products, Apps, on-premises
2. **Logs** - collects logs as above
3. **Events** - Cloudwatch can generate events to do something

- Namespace is a container for monitoring data. It's a way of separating things into different areas e.g. AWS/EC2
- A metric is a collection of related data points in a time ordered structure e.g. cpu utilization, network I/O or disk I/O
- Data points are measurements of data consisting of a timestamp and value
- Dimensions are used to separate data points within the same metric e.g. instance ID (i-xxxxx) and instance type (t3.small)
- We can take action on metrics using alarms

### Shared Responsibility Model

- Shared responsibility in AWS is the principal that some areas you have to manage vs AWS have to manage
- At a high level, AWS is responsible for the security of the cloud where as customers are responsible for the security in the cloud
- AWS responsibilities include managing security of regions, Availability Zones and Edge locations specifically the hardware/global infrastructure.
- AWS also manage the security around compute, storage, database and networking as well as any software that is used to provide those services
- Customers need to take care of client side data encryption, server side encryption and network traffic protection.
- Customers need to take care of OS, network and firewall configuration
- Customers need to take care of platform, applications, identity and access management as well as customer data.

![Shared Responsibility Model](/assets/images/srp-model.png)

### High-Availability vs Fault-Tolerance vs Disaster Recovery

- High Availability (HA) aims to ensure an agreed level of operational performance, usually uptime. for a higher than normal period
- HA is about maximizing a system's online time
- System availability is usually expressed as a percentage of uptime e.g. 99.9% a year means 8.77 hours p/year downtime
- Fault tolerance (FA) is the property that enables a system to continue operating properly in the event of the failure of one or more of its components
- HA is just about maximizing uptime where as FA is operating through failure e.g. a aeroplane can't just be highly available it must be fault tolerant
- FA is much more complex and more costly to implement as you need to minimize outages but also design a system that will tolerate a failure
- Disaster recovery (DR) is a set of policies, tools and procedures to enable the recovery or continuation of vital technology infrastructure and systems following a natural or human-induced disaster
- You need to plan what should be done in the event of a failure. An example of DR planning is having off-site backup storage
- DR planning should happen in advance so that the process is automated

**Summary**:

- HA - minimise outages
- FA - operate through faults
- DR - used when these don't work

### Route53 (R53) Fundamentals

- Route53 provides two main services:

1. Allows you to register domains
2. Hosts zones on managed nameservers it provides

- Route53 is a global service with a single Database. You don't need to pick a region
- It is globally resilient so it can tolerate the fault of multiple regions
- Route53 provides DNS zones as well as hosting for those zones
- Zone files are created and hosted on four managed name servers
- Hosted zones can be public or private (VPC)
- A hosted zone hosts DNS records (recordsets)

### DNS Record Types

- There are different records that can be stored in DNS:

  - Nameserver (NS) - allow delegation to occur end to end in DNS. e.g example.com → ns1.example.com, ns2.example.com

  - A and AAAA Records - A record will point to a v4 IP address and the AAAA will point to the v6 IP address. For example:

    ** A Record:** example.com → 192.168.1.1

    ** AAAA Record:** example.com → 2001:db8::1

  - CNAME - Canonical Name - lets you create the equivalent of DNS shortcuts by pointing to the same A record. E.g. www.example.com → example.com
  - MX Record - how a server can find a mail server for a specific domain. Includes a priority number
    ```
    example.com
      Priority: 10 → mail1.example.com
      Priority: 20 → mail2.example.com
    ```
  - TXT - allow you to add arbitrary text to a domain that must be matched to prove domain ownership.

- TTL (Time To Live) is the time set by the DNS to determine how long a DNS record is cached by a resolver (DNS server o browser) before it must check for an updated record from an authoritative server.

## IAM, Accounts and AWS Organisations

### IAM Identity Policies

- IAM policies are a type of policy which get attached to identities in AWS
- Identities are IAM users, groups and roles
- IAM Policies:

  - provides and denies access to features in AWS
  - Policy documents are created using JSON containing one or more statements
  - the first part of a statement is a Sid (Statement ID) which is an optional field that lets you identify a statement and what it does. Using these is best practice to inform the reader
  - Every statement will have a resource you're interacting with and the action you're wanting to perform on that resource
  - The action is in the format "service:operation" where the operation can possibly be a wild card or a list of multiple actions
  - Resources is the same only it matches AWS resources. Individual resources are referred to using the ARN
  - Effect is either allow or deny. It is possible to be allowed and denied at the same time

  ```json
  <!-- Policy document example -->
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "FullAccess",
        "Effect": "Allow",
        "Action": ["s3:*"],
        "Resource": ["*"]
      }
    ]
  }
  ```

  - when there is an overlap in permissions, then both of the statements are processed where the priority begins at explicit denies. Denies overrule everything else. The second priority are explicit allows. Allows take effect unless there are explicit denies. The default if no rules are in place, the default is DENY.
  - With the exception of the account root user, aws identity start of with NO ACCESS to aws resources.
  - Remember: explicit DENY > explicit ALLOW > DENY

- There are two types of policies: Inline policies and managed policies
- Inline policies are when you apply individual JSON policy documents to each individual account. This is good for exceptional or special access rights for an individual as opposed to a group or a number of people
- Managed policies are another JSON policy that you'd attach to identities in a reusable way. These should be used for the normal default rights in a business as they are low overhead
- There are two types of managed policies: AWS managed policies and customer managed policies which you can create and manage for exact requirements

### IAM Users and ARNs

- IAM users are an identity used for anything required long term AWS access e.g. humans, applications or service accounts
- A principal (a person/application) makes a request to IAM to authenticate to a resource
- Authentication for IAM users is done using either username and password or access keys. Access keys are usually used by applications or by humans using CLI tools.
- Once a principal goes through the access tools, they become an authenticated identity
- Once a principal is identified AWS knows which policies apply to an identity. This is the process of authorization.
- Authentication is how a principal can prove to IAM it's who they say they are where as Authorization checks the policies attached to the identity to give them permission for a resource
- ARN (Amazon Resource Name) uniquely identify resources within any AWS accounts.
- ARN is used to allow you to refer to a single or group of resources using wild cards
- ARNs are used in IAM policies
- The format is:
  `arn:partition:service:region:account-id:resource-id`
  `arn:partition:service:region:account-id:resource-type/resource-id`
  `arn:partition:service:region:account-id:resource-type:resource-id`
- You can only have 5000 IAM users per account
- An IAM User can be a member of 10 groups
- If you have more than 5000 identifiable users then IAM users is not the right identity to use for that solution. You can fix this with IAM roles or Identity Federation.

### IAM Groups

- IAM groups are containers for IAM users
- You can't log into IAM groups nor do they have credentials of their own
- They are used solely to manage and organise IAM users
- an IAM user can be part of multiple IAM groups
- Groups can have policies attached to them, both inline and managed
- You can also have individual inline/managed policies at the user level
- You should collect all the policies that apply to a user from their groups and individual policies and apply the same deny-allow-deny rule to work out what their permissions are
- There is no limit for the amount of users in an IAM group but the IAM user limit of 5000 exists for the whole account
- There is no such 'all users' group in IAM built in. You can create this and manage it manually
- You cannot have any nesting in groups
- There is a limit of 300 groups per account but it can be increased with a support ticket
- Policies can be attached to resources as well for example a bucket can have a policy attached to it where it allows and denies identities access to that bucket.
- A resource can be refer to a user or role to give permission to itself but it cannot give it to a group. This is because a group is not a true identity and they can't be referenced as a principal in a policy

### IAM Roles

- A role is a type of identity that exists inside an IAM account
- IAM user is when a single principal wants to use AWS
- IAM roles are best suited to be used by multiple principals e.g. multiple users in the aws account or users, apps or services inside or outside of the aws account.
- If you can't identify the number of principals that use an identity or if you have more than 5000 principals you could consider using an IAM role
- Usually roles are used on a temporary basis to borrow permissions
- IAM roles have two types of policies that can be attached:

  1. Trust policy - which identities can assume that role. This can be entities in AWS accounts, other accounts, anonymous users and SSO providers e.g. facebook, google etc
  2. Permissions policy - temporary credentials are given to identities assuming the role and these credentials are used to check the permissions

- temporary credentials are generated to roles using STS (Secure Token Service).

### When to use IAM Roles

- One of the most common uses of IAM roles is AWS services as they need permission and access rights to perform certain actions
- An example is AWS Lambda - it may start/stop ec2 instances, perform backups or other tasks that need permission
- Instead of hardcoding the access keys into the Lambda, the IAM role 'Lambda execution role' can grant access to aws product/services. It will use the sts:AssumeRole operation to generate temporary credentials to use AWS services.
- This is a better approach than using access keys as it's more secure and it won't need key rotation
- Roles are also useful for emergency or unusual situations. A person can assume an emergency role when absolutely required for a short time.
- Roles are useful for an existing corporate environment. If a corporate has over 5000 staff you cant assign each of them an IAM user.
- You could allow an IAM role inside your AWS account to be used by an external identity e.g. active directory
- If you create an app with over 5000 users that needs AWS access, you can use web identity federation which uses IAM roles. Web identities can be providers such as google, facebook or twitter/X
- The pro of using web identities is that no AWS credentials are stored on the app and it uses existing accounts that customers have. This can scale to 100 million+ users
- If you want to use resources across aws accounts, aws roles can also be used.

### Service-linked Roles and PassRole
- Service-linked roles are a special time of IAM role linked to a specific AWS service
- their permissions are pre-defined by an AWS service
- The main difference between a regular IAM role and a service-linked role is that you cannot delete a service-linked role until it's no longer required
- They are either created by a service or by you during set up
- Passrole permissions give the users the ability to use a service linked role without being able to create or edit the role. This is similar to using a pre-created role in a cloud formation stack. 


### AWS Organisations
- AWS organisations take a single AWS account (standard account) to create an organisation. This account becomes the management account (previously called Master account). 
- The management account invites existing standard AWS accounts into the organisation. Once they join they change from being standard to member accounts.
- An AWS organisation has only ONE management account and zero or more member accounts
- An organisational root is not the same as an AWS account root user. The root of an AWS organisation is just a container for aws accounts and organisational units. It's the top level of the hierarchical structure of an organisation. 
- Consolidating billing for organisations changes the billing methods for member accounts by removing them and passing them through to the management account. In the context of consolidated billing this is known as the Payer Account. 
- Master, management and payment account refer to the same thing - the account that was used to create the organisation. 

### Service Control Policies

- SCPs are a feature of AWS organisations which allow restrictions to be placed on member accounts in the form of boundaries
- SCPs are policy documents or JSON that can be attached to the organisation as a whole or organisational units or individual AWS accounts
- They inherit down the organisation tree so that nested units will be affected by it and everything below it will be affected too
- Management accounts are special as they cannot be restricted and not affected by service control policies.
- They can limit what a root user can do though
- SCP do not grant permissions. They just control what an account can and cannot grant via identity policies
- By default, SCP applies FullAWsAccess which means no restrictions 
- SCP also has implicit deny if there is an absence of an allow
- Only permissions allowed within the intersection of Identity policies and SCPs are allowed

![SCP and Identity Policies Venn](/assets/images/SCP-policies-venn.png)

### CloudWatch Logs
- A public service that allows you to store, monitor and access logging data
- Has built in AWS integration with services eg EC2, VPC, Lambda, CloudTrail, R53
- Can generate metrics based on logs (metric filter)
- Log events are stored in log streams. Log streams are from one specific source e.g. one ec2 instance
- Log groups are containers for multiple log streams for the same type of logging 
- Log groups are where we define retention and permission policies and metric filters
- Metrics can have associated alarms

### CloudTrail Essentials
- A product which logs API calls and account events/activities e.g. creating, deleting s3 bucket, stopping a service etc
- A cloudtrail event is a call/activity on an aws account
- Stores 90 days of event history - enabled by default for no cost. You don't get any s3 storage unless you configure a trail. 
- To customise this you must create 1 or more Trails
- Management Events and Data Events are the type of trails
- Management Events are control plane operations 
- Data events are resource operations e.g. uploading objects, lambda functions being invoked
- By default, cloud trail only logs Management events
- A trail logs events for an AWS region it's created in
- Cloud trail is a regional service
- A trail can be set to all regions or one region 
- By default, regional trails will log to the region they're in but global services will log to **us-east-1**
- If you create a trail, it is stored in an s3 bucket as compressed JSON log files
- CloudTrail could also be integrated into Cloudwatch logs
- You can create an organisational trail which is a single management point for every event across the whole organisation 
- CloudTrail is NOT real time. There can be a 15+ minute delay

### AWS Control Tower
- AWS Control Tower gives an easy and quick way to set up a multi-account environment
- Control Tower orchestrates other services to provide this functionality e.g. Organizations, IAM identity center, cloudformation, config
- It's another evolution of AWS Organisation with more capability 

There are a few different parts of control tower:
- Landing zone - multi-account environment 
- Guard Rails - detect/mandates rules and standards across all accounts
- Account Factory - automates and standardises new account creation 
- Dashboard - single page oversight of the entire environment 

**Landing Zone**
  - Home region - the region you deploy the region 
  - brings features of multiple AWS products together e.g. Organizations, AWS Config, Cloudformation
  - Security OU - organisational unit that has log archive and audit accounts
  - Sandbox OU - which is for testing and less rigid security
  - You can create other OU's and Accounts
  - Utilises the IAM Identity Center (AWS SSO) - SSO, multi account, ID Federation 
  - Monitoring and Notifications - cloudwatch and SNS

**Guard Rails** 
 - Come in either Mandatory, Strongly Recommended or Elective
 - Function in two ways
  1. preventative - stop you from doing things - either enforced or not enabled
  2. detective - compliance check for identifying issues - either clear, in violation or not enabled 

**Account Factory**
  - Automate account provisioning
  - Can be done with cloud admins or end users with appropriate permissions
  - This provisioning comes with Guardrails which are automatically added
  - Account admin given to a named user to allow people in the organisation to provision accounts
  - Accounts are set up with standard configuration
  - Accounts can be closed or repurposed
  - Can be fully integrated with a business SDLC

## Simple Storage Service

### S3 Security (Resource Policies and ACLs)
- S3 is private by default 
- The only identity which has any access to s3 by default is the account root user
- You can grant permission to s3 via S3 Bucket Policies:
  - A form of resource policy 
  - like identity policies but attached to a bucket
  - Resource perspective permissions - you control who can access that resource 
  - Identity policies are limited by only being able to give control to the current account, so you cannot give another account access to an s3 bucket. Resource policies allows this for the current or different accounts. 
  - Resource policies can ALLOW/DENY anonymous principals. This can't be done with identity policies since they need to be attached to a valid identity in AWS. Therefore the resource policies can be given to external access.  
  - Resource policies have a 'principal' component which specifies which principals this policy applies to
  - An identity policy doesn't have a principal because the policy always applies to the account that created it. A good way to identify identity vs resource policy is checking the absence of principal
  - Identity policy as well as the bucket policy applies to both internal and cross account access. For anonymous only the bucket policy applies. 

Access Control Lists (ACLs):
- Another form of s3 security used less frequently these days. They aren't recommended anymore by AWS
- A sub-resource of an object or bucket.
- You cannot use ACLs on a group of objects 

Block Public Access:
- Another layer of permission to block all public access as a fail safe 

Choosing between resource or identity policies depends on the business' requirements and personal preference but sometimes choosing one over the other makes sense in specific situations:
- If you're granting/denying permissions on lots of different resources across an aws account, then you will need to use identity as not all services support resource policies. 
- If you prefer to manage resources from one place then identity makes sense because this is done in IAM
- If you're managing a specific product then resource makes sense
- Cross account or anonymous resources should use resource policies
- Do not use ACLs unless you MUST  

### S3 Static Hosting
- Without static hosting, you access S3 Via AWS APIs
- With static hosting, you can access those resources via HTTP e.g. websites, blogs etc 
- You must set an Index and Error document for s3 hosting. We point the index document to a specific html file object in the bucket as well as Error
- A website endpoint is created for hosting
- You can use a custom domain via R53 but the bucket name MUST match the domain name

There are two scenarios that are perfect for s3
1. Offloading - moving large data from a compute service to s3 is cheaper
2. Out-of-band pages - if a server is offline for maintenance or has performance bugs we can point users to a static page on s3 that contains something like a status message

Pricing:
- Per GB per month charge
- Transfer fee - in is free, out is per gig charge
- Request and data retrieval - every operation e.g. GET, POST incurs a cost


### Object Versioning and MFA Delete
- Disabled by default. **Once enabled you cannot disable it** 
- You can suspend it and a suspended bucket can be re-enabled
![S3 versioning state](/assets/images/s3-versioning-state.png)

- Versioning let's you store multiple version of objects within a bucket where as without it, there is a unique object name and the object gets replaced each time.
- the 'id' is null if versioning is disabled, if it is enabled then s3 will allocate an id 
- newer versions will have a new ID
- The newest version is known as Latest or current version
- If you don't specify to s3 a specific version, you always get the latest. You can access individual versions by specifying the ID
- If we delete an object, the object is not deleted, it's just hidden and  marked with a delete marker. This is when we don't specify an ID/version
- if we specify a version ID then an object is actually deleted 
- Important: **versioning CANNOT be switched off - only suspended***
- Space is consumed by ALL versions and you are billed for ALL versions
- Only way to remove all costs is to delete the bucket. Suspending does not remove the old versions
- MFA Delete - enabled in versioning configuration which means you need an MFA token to change bucket versioning state or delete version
- You would need to pass in MFA and the code passed with API calls to do these actions

### S3 Performance Optimization
- Default upload in s3 is using single data stream to s3. The issue with this is if the stream fails the entire upload fails and you need a full restart
- A single PUT upload in AWS is limited to 5GB as a maximum 
- A solution to a single stream is using a **multi-part upload**
- This breaks the original blob into individual parts
- The **minimum** data size to use multi part upload is 100MB. It's recommended to use it for anything over 100mb. 
- An upload can be split into a maximum of 10,000 parts with each part between 5mb -> 5gb
- Each individual part is treated as it's own isolated upload and can be restarted in isolation 
- It also improves transfer rates by uploading in parallel 
- S3 Accelerated transfer 
  - using the public internet is not the most ideal way to get data from source to destination as the route chosen by ISPs is not always optimal. S3 transfer acceleration uses the network of AWS Edge locations located in convenient areas. This feature needs to be switched on. 
  - Edge locations then use AWS Network as an 'express train' to get to the destination