# Amazon Web Services

Web application system design architecture can vary based on vairous requirements. Some of the ways are

- Static Sites - optional backend in serverless lambda - jekyll site.
- Containers - using docker images, or Heroku.
- VMs - fully available OS.
- 3 tier web app, monolithic app, modular app or a group of webserives.

## AWS Overview, Infra and Access

**AWS Global Infrastructure** - AWS provides various services on cloud which can be used to build these systems. Is is PaaS. AWS Global Infrastructure has multiple `servers`, in multiple `data centers`, in multiple `availability zones`, in multiple `regions`. This is how AWS makes data available and disaster proof. Regions are geo bound. Pick a region based on

- Compliance - app might be restricted to region or global
- Latency - server should be close to end user or business
- Pricing - due to tax price vary in different regions.
- Availability - not all services are in all regions.

![AWS Global Infra](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661436000/8wO7QUlpnj0--chB_E8Evg/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/P5Q7lnLBa5D8kQS__mlutDPn7u3ksQ4cs.png)


**Interacting with AWS** - as it is Virtual, on cloud, hence you need API to manage services. API is available as:

- AWS Console - a GUI, web based to login and manage the serives. Click based. Region based. Interactive forms to use services.
- AWS CLI - a Command Line Interface that is scriptable.
- AWS SDKs - a Software Development Kits, eg Python, Java etc. Useful if you want to stay in dev language env. Eg, if you app is using Python and Flask, then you can use Python to interact with AWS services and host the app.

**Security and the AWS Shared Responsibility Model** - AWS secure the cloud, you secure in the cloud. You secure your data, firewalls, access by users, encryption etc. Each AWS service has its own security model. It has followig credentials types:

- One is username and password, that can be used on web
- The second set of credentials is called access `keys`, which allow you to make programmatic requests from the AWS Command Line Interface (AWS CLI) or AWS API. Access keys consist of two parts:
  - Access key ID, for example, A2lAl5EXAMPLE
  - Secret access key, for example, wJalrFE/KbEKxE

As best practice, do not use root user (AWS email ID) for day to day task. Protect the AWS Root User as it has unrestricted access to everything in account. Also add `Multi Factor Authentication` MFA, which enables added security like RSA.

Use `IMA Account` to do actions as it has restricted access.

**AWS Identity and Access Management**

- EC2 needs access and permission to talk to S3, so all services need authentication and signed api calls in each request.
- Each developer can have `IAM account`, this `authenticates` (access). Then each user needs `authorization` (permission) to different services.
- `IAM policies` can be used to grant or deny permissions to users to take actions. Actions are basically API calls, everything in AWS is API calls.
- Policies are JSON documents. JSON defines which service has, what level of access with some conditions. Basically you can control every single thing by creating this policy. Policy is basically rule. Policy must have effect, action and resouce.
- `Policy` can then be attached to a IAM identities, `User` or `Group` to inherit.
- Policy JSON Example:

    ```json
    {
    "Version": "2012-10-17",
    "Statement": [{
    "Effect": "Allow",
    "Action": "*",
    "Resource": "*"
    }]
    }
    ```

Role-Based Access in AWS

- IAM Roles are like IAM users that have association with IAM Policies and have auth tokes.
- Roles are used by services to talk to each other. Eg, you can give a role to EC2 machine to read and write to S3 bucket and to RDS and DynamoDB.
- Role can be associated to policy.
- For Corps, you have SSO and Identity.

## AWS COMPUTE

Compute as a Service

- web server, batch job, ML; all need compute. Compute mainenaince needs time.
- AWS offers compute as:
  - Virtual Machines
  - Containers
  - Serverless

Amazon Elastic Compute Cloud

- EC2 provides various types of instances which are for different purpose, like web server, graphics server.
- Amazon Machine Image - AMIs can be installed on EC2 machines.
- EC2 machines can be scaled up in no time.

Amazon EC2 Instance Lifecycle

- EC2 is charged when running or rebooting. Once terminated, they are deleted forever.
- To update the VM, duplicate the VMv1.0, make updates to clone, the switch the app to VMv2.0, terminate v1.0

Container Services

- containers provide efficiency and portability
  - `ECS Elastic Container Service`
  - `EKS Elastic Kubernetes Service`
- They both run on top of EC2, henc use EC2 as a service.
- Container orchestration tool help us manage 100s of containers easily

Serverless

- You will not manage the updates and patches to the server os. Instead server is completely abastracted from you. You need not care about scalability, updates or other server management.
- `AWS Fargate` is used for this. No need to worry about underlying OS or Env.
- EC2 gives more control while Fargate gives more conveniece but less control.

AWS Lambda

- Serverless compute. Package and upload code as lambda function
- Not running all time. runs when triggered.
- List of triggers exit, like HTTP, upload of file, events from other AWS services or inbonile activity.
- Runs on managed service, it is scalable.
- You can choose, env, os, size memory etc.
- all lambda runs in their own env.
- Not for Wordpress site, but for smaller web services or task, eg, resize photo to thumbnail. Lambda is billed only when function runs, upto 100ms interval. So you don't need image resize service to always run, but only when photo is uploaded.
- Create a lambda function, add a role to it, if it needs to access other AWS service.
- Add a trigger to invoke lambda function.
- Upload code to lambda function.
- See it invoked in CloudWatch
- [AWS Lambda](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661436000/8wO7QUlpnj0--chB_E8Evg/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/2Y0EEVpXidSfgdKp_WFR-TreKi3bqKSvu.jpg)

Choose the Right Compute Service

- Prototype on permise app - EC2
- One a quarter or month file data wrangling - Lambda
- Micro services that need regular updates - ECS or EKS

For compute in AWS, the three most commonly used services are as follows:

- Compute on instances - EC2
- Container services
- Serverless services

`Amazon EC2` are virtual server instances in the cloud. Amazon EC2 gives you complete control over the instance, down to the root level. You can manage the instance as you would manage a physical server. You can use instances for long-running applications, especially those with state information and long-running computation cycles.

`Amazon ECS, Amazon EKS` Container management services that can run containers on either customer managed Amazon EC2 instances OR as an AWS managed serverless offering running containers on AWS Fargate. Before software is released, it must be tested, packaged, and installed. Containers provide a standard way to package your application's code, configurations, and dependencies into a single object. Containers run on top of the host OS and share the host's kernel. Each container running on a host runs its own isolated root file system in a separate namespace that may include it’s own OS. They are designed to help ensure quick, reliable, and consistent deployments, regardless of the environment. Containers are useful when taking a large traditional application and breaking it down into small parts, or microservices, to make the application more scaleable and resilient. When not to use containers? When applications need persistent data storage.

`AWS Lambda` is Serverless compute for running stateless code in response to triggers. Using AWS Lambda, you can run code without provisioning or managing servers. You pay only for the compute time you consume. There is no charge when your code is not running. With Lambda, you can run code for virtually any type of application or backend service without provisioning or managing servers. Upload your code, and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to be automatically invoked from other AWS services or call it directly from any web or mobile app. Lambda is a suitable choice for any short-lived application that can finish running in under 15 minutes.

What AWS Compute service to choose?

- something that can **build fast** and let you **hit the market** so you can analyze and see if it works
- put time in - business logic and data processing logic.
- do not waste time on infra concern like load balancing, scaling, networking; or plumin code like logging, authenticaiton, caching exceptions so on.
- use serverless architecture using lambda, s3, cloudfront, step functions, congnito, appsync, dynamodb. They are all scaled, available and charged per request basis.
- just define logic as lambda function, and invoke it via a respond to api call or an event.



## AWS NETWORKING

Networking

- Amazon `VPC - Virtual Private Cloud` is network configuration, same as modem and router in physical world. In EC2, instead of default config, we can use custom VPC configuration
  - to add `more security`, like only allow HTTP on a certain port. Hence, no SSH on 22.
  - to give different `access control` to different resources, like public/intranet/private.
  - to achieve `high-availability` and fault tolerance by associating different AZs. Compute is replicated. we will have more than one EC2 hence more than one VM.

Amazon VPC

- CIDR notation is used to provide variable IP address or range of IP addressess. Eg, `192.168.1.0/24`. /16 is more, /24 is less.
- `VPC` gives you range of IPs. `10.1.0.0/16`. You need to create VPC in AWS management console - GUI.
- Then you can use these IPs to create `subnets`, which uses some IPs of VPC-IPs to make private or public network. Subnets are associated to AZs - Availability Zones. Say in Zone-A
  - Public resources, or internet facing resources, are added to `public subnet` with a sub-range of the VPC IP range, eg, `10.1.1.0/24`. like web-app
  - Private resources are kept in `private subnet` with a sub-range of the VPC IP range, eg `10.1.3.0/24`. like database.
- To expose public subnet to internet, we need `IGW - Internet GateWay`, this is just like a modem. Create internet gateway and attach it to your VPC.
- To only expose the subnet to corporate intranet or VPN, create `VGW - Virtual Private GateWay`. This will expose the AWS to on-premise data center.
- to make it always available, duplicate the subnets and add to another AZ.
  - ![VPC on AWS](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661360400/IoAR-JfBleLFS717fAPYjA/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/wPxuV_0JWkhj9dQV_InIjhnQ0x-d4xLbK.png)

Amazon VPC Routing

- When a user reached Internet Gateway, it needs to be routed to correct subnet. For this we need routing table.
- AWS create default `main route table`. This provides local trafic only.
- GUI - each VPC have routes.
- We have called subnet public/private however, that is implemented by routes, which controls the exposure of the subnets.
- Edit route table, add new route and add destinatoion `0.0.0.0/0` that takes and servers all IPs. then add internet gateway to it. finally, associate it to public subnets.
- Later add firewall for extra security.
- ![AWS Route Tables](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661360400/IoAR-JfBleLFS717fAPYjA/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/TOFyTY8NnEvCzqi3_NrsH76kCPC3B5ySy.jpg)

Amazon VPC Security

- Subnets can be made more secure, like only allow HTTPs to inbound and outbound traffics on port 443.
- To do this create rules in `Network ACLs - access control lists`. Like firewalls.
- Secondly, `security groups` can provide more security to EC2instances.
- ![AWS VPC Security Groups](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661360400/IoAR-JfBleLFS717fAPYjA/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/FgEhatXf6qnJQ1X1_scxJIDNUzFAqjZbL.jpg)

Creating EC2 with VPC configuration

- to implement this we need to do following
  - create `elastic IP` address for NAT Gateway, NAT gives connectivity to private resources not exposed to internet.
  - create a `VPC`
  - then make `subnets` pub/pvt, and associate them with different AZs.
  - add `NAT gateway` for connectivity to pvt resources.
  - add Internet Gateway to expose public subnet to internet.
  - create `route-table` to route IP traffic to specified gateway NAT/IGW. To associate pub/pvt with NAT/Internet-GateWay. Create two route-tables. then add NAT/Internet gateway. then associate with subnets.
  - `associate subnets` to route tables.
  - create `security group` to allow/block certain protocols (HTTP/SSH) and ports(22/8080).




## AWS STORAGE

Storage Types

- we need to store
  - OS - application files, ubuntu
  - Static Data - files - employee photo - write once, read many WORM
  - Structured Data - database tables

- Block Storage - splitted into chunks - one char change in 1gb file is simple - System File, Log File.
- Object Storage - single unit - one char change in 1gb file, whole file si rewritten - WORM, Video

Amazon EC2 Instance Storage and Amazon Elastic Block Store EBS

- EC2 has `instance` storage like internal HDD. This is block-level storage. attached physically, hence fast but tied to lifecycle of EC2.
- EBS is like external HDD
- EBS can be linked to one EC2 or multiple EC2 can read-write.
- EBS can be HHD or SSD
- EBS can be snapshotted to keep backup.

Object Storage with Amazon Simple Storage Service S3

- EBS is not fit for all as
  - they are mostly 1-1 with EC2
  - have size limit.
- S3 is scalable, standalone, not tied to EC2, not mounted
- access by URL
- max 5TB file
- is object storage
- has flat structure
- unique bucket name
- it is region constrained
- is durable and available as it is auto distributed in a region
- objects are: buckets/folders/files
- Demo
  - search s3 - create bucket - region specific - bucket has a URL
  - upload objects - files
  - files have URL

- bucket/folder/files are private and not public to world.
- can be made public using actions, but only in public bucket
- for access control use
  - IAM, and
  - `S3 bucket policies`, it has same json format and attached to buckets
- actions allowed or deny. can be read only, another acct write.
- Uses: static websites, data lakes, media, backup and storage.
- [AWS S3 permissions](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1661436000/8wO7QUlpnj0--chB_E8Evg/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/tdqWRVkjrqaKcAF3_0M9ExB6uo3T8oZq0.png)

Choose the Right Storage Service

- EC2 Instance store is generally well-suited for temporary storage of information that is constantly changing, such as buffers, caches, and scratch data
- Amazon EBS, block storage, is meant for data that changes frequently and needs to persist through instance stops, terminations, or hardware failures.
- If your data doesn’t change that often, Amazon S3 might be a cost-effective and scalable storage solution for you. Amazon S3 is ideal for storing static web content and media, backups and archiving, and data for analytics. It can also host entire static websites with custom domain names.


## AWS DATABASES

Databases on AWS

- RDS for data.
- App can connect to DB on your on-premise server. you manage everything. Or
- DB on ec2, then have to install DB s/w, manage updates, backups and replicate it on instances for high-availabiility. Or
- Managed AWS database service, you only code. AWS manages everything else.

Amazon Relational Database Service RDS

- RDS > create database > easy create
  - select engine > mysql/mssqlserver/postgres
  - BD instace size > free
  - identifier name
  - username
  - password
  - create db.
- RDS is in one pvt subnet. to make high-availability, create secondary RDS in another AZ subnet, using `RDS Multi-AZ deployment`. RDS will manage replication.
- oneis primary another secondary. fail overs handeled by rds.

Purpose-Built Databases

- Relational databases ae good to manage complex schemas, which have joins and complex queries and stored procs. However, this adds overhead in engine.
- There are other DBs which work best to query single records.
- Also RDS is charged per hour, you query or not. weekedn
- dynamo, key value, docs, non relatin, ms larency, usage charge and amount.
- amazon docDB - content
- social n/w - graph db - az neptune, recommendation, fraud detection ??? todo
- finance, ledger, aamzon qldb, immutable, audit , compliance
- these are purpose built.

Amazon DynamoDB

- fully managed NoSQL database, serverless
- no relations
- scalable, performant, under ms time
- tables, items, and attributes are the core component.  A table is a collection of items, and each item is a collection of attributes.
- access via dynamo db api
- search dynamo
- create table
- unique identifiedr
- table name, primary kry
- items to view items

Choose the Right Database Service

|Database Type  |Use Cases  |AWS Service
-|-|-
Relational      |Traditional applications, ERP, CRM, e-commerce|Amazon RDS, Amazon Aurora, Amazon Redshift
Key-value       |High-traffic web apps, e-commerce systems, gaming applications|Amazon DynamoDB
In-memory       |Caching, session management, gaming leaderboards, geospatial applications|Amazon ElastiCache for Memcached, Amazon ElastiCache for Redis
Document        |Content management, catalogs, user profiles|Amazon DocumentDB (with MongoDB compatibility)
Wide column     |High-scale industrial apps for equipment maintenance, fleet management, and route optimization|Amazon Keyspaces (for Apache Cassandra)
Graph           |Fraud detection, social networking, recommendation engines|Amazon Neptune
Time series     |IoT applications, DevOps, industrial telemetry|Amazon Timestream
Ledger          |Systems of record, supply chain, registrations, banking transactions|Amazon QLDB|


## AWS MONITORING, OPTIMIZATION, SCALING

Application Management

- how is app performinh, how is it usitilizing resoirces
- sacalability - demand not constand, add and reducse esources
- balance traffic

Monitoring

- important to see how services are being used
- monday morning latency, not good to take tickets
- proactive before end user notify
- and where is the problem? recent code change, db or ec2?
- metrics, logs, traffic, db connections, cpu usage; need to be monitored
- monitoring tool help in this by analyzing, metrics over time, called statistics. Based on stats there can be triggers.
- all info need to be on centrol console, hence cloudwatch
- all in one place

Amazon CloudWatch

- users can create `dashabord` from various granular metrics available
- can create `alarms` to be raised on crossing a basevalue over a time
  - this can trigger `SNS`, Simple Notification Serive, which cretes a topic. Any one subscribed to the `topic` will get notified.
  - can trigger a EC2 boot action,
  - can sacle up resources
- user can also send custom app metric, like 'page load time' which not aws metric but app specific, but can be seen on CloudWatch and can trigger alarms.

Solution Optimization

- we need to optimize infra
  - capacity - storage - s3 - mostly autoscaled
  - peformance - ec2 and db, db mostly autoscales, but ec2 has capacity
  - availablility - manage traffic and make all services available.
- prevent or repond so as to avoid bottle neck.
- to increase availability increase redundancy, or replication of services
- `AutoScaling` lets you automatically add and remove instances.
- in horizontal scaling the problem is that each EC2 has its own public IP address and the traffic needs to be sent to one available. To solve this issue, we use a `load balancer`. It removes need of public ip addresses for each EC2.


Traffic Routing with Amazon Elastic Load Balancing

- now we have multiple ec2 in public subnets.
- request from browser - goes to load balancer - sends to ec2.
- ELB, `Elastic Load Balancer` does the traffic management. Highly available in each zone.
- ALB - `application load balancer`, hanfles Http or Https.
- ALB needs, `listener`, port and protocaol
  - `target` group - which backend resource to send to, ec2 s3 dynamo
    - it sends the trffic based on helath of backend
  - `rules` - how to divert a traffic. it is default as well as we can define rules for pages, apps etc.
- Other is `Network Load Balancer` supports TCP, UDP, and TLS protocols.
- ![ELB Load Balancer AWS](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1662030000/t25JDfL13ox0tFPZBJWcwQ/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/bm6CZb422WTOZvIM_sy34Df66d1mbugqD.jpg)



Amazon EC2 Auto Scaling

- we have 2 instances, but demand may inc.
- instead of adding instances manually, use EC2 autoscaling, more capacity based on threshold in cliudwatch.
- ec2 cpu goes up, cloudwatch triggers alarm, autoscale is asked to give more ec2 instances. each is added to ALB with health checks amd thus high horizontal scalability. hecnce CPU down across fleet.
- Three main components of EC2 Auto Scaling are as follows:
  - Launch template or configuration: What resource should be automatically scaled?
  - EC2 Auto Scaling Group: Where should the resources be deployed?
  - 'Scaling policies: When should the resources be added or removed?

- ![AWS EC2 Autoscaling Templates](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1662030000/t25JDfL13ox0tFPZBJWcwQ/tincan/d03722b85f9d2b3a05e4c74bd586ea9b1f52f81a/assets/Y9U8MWYOjosNS6Ta_0tIEHgm6nFcjy6rI.jpg)





## AWS Other Services

`AWS Lighsail`  - easy VPS hosting. Quickly launch and manage OS with configued Dev Stack (like Ubuntu with LAMP). Add load balance, firewall and dns. Once requirements increase, easily move to EC2 or Lambda. Lightsail provides low-cost, pre-configured cloud resources for simple workloads just starting on AWS. Amazon EC2 is a compute web service that provides secure, resizable compute in the cloud. It has far greater scale and optimization capabilities than Lightsail.

`AWS Batch` lets you do batch jobs, by giving right cpu gpu and memory.

`AWS Elastic Beanstalk` - easy devops for 3 tier app easy devops. gives url for app. free, only pay for aws services. api, web app etc.

`AWS CodeStar` is development tool to develop, build and deploy app on AWS.

`AWS Amplify Console` provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser.


## DEMO - Handson Implementaion - Build Employee Directory App

We will create basic CRUD app, employee directory app on AWS.

Architecture:

- App is hosted on Private Network using, `VPC - Virtual Private Cloud`.
- Backend on `EC2 - Elastic Compute`, which is VM on AWS. App code is here without the data or files. hence this can be replicated to achieve high availability.
- Data is stored in database in same NW, either `RDS - relational data service` or `DynamoDB - key value`.
- Images in `S3 - simple storage service`.
- Monitoring using `CloudWatch` for health monitoring.
- Scalability using `ELB - Elastic Load Balancer`. This will distribute load on the available servers (instances). Also `EC2 AutoScaling` will help scale out or in based on demand.
- `IAM - Identity and Access Management` for security and access amangement.
- `AWS Management Console` - to build all this.



Demonstration: Implement security with AWS Identity and Access Management (IAM)

- Create Groups, then add Policies to it.
- Add Users to the Groups.
- Eg, create group to read-only ec2 state, or read-only s3.

Employee Directory Application Hosting

- Create a EC2 machine
  - Use default VPC, this is network. each ec2 has to be in a network
  - Choose Role for this machine, eg, EC2 to have full access to S3 and Dynamo DB.
  - User Data: Add script to run when this machine boots. This is basically linux 'profile/env' info, like exporting variables and setting paths. Additionally it can have, download code, unzip, install requirements, set flask app path, run the app.
  - Configure Security Group - this is to allow HTTP requests to your machine, by adding security groups.
  - Finally launch. Once launced, you will get a public IP address, this will let you access the app.

EC2

- Use `Amazon Linux 2 AMI`, select `t2.micro` free tier insatance.
- Add script, that will:
  - update all
  - install node
  - create app dir
  - download and unzip app code
  - install dependencies
  - run the app, `npm start`
- add storage, 8gb is enough.
- securty group, acts as virtual firewaal that controls tha ccess. we need web traffic for app and ssh for management. HTTP allows inbound traffic on port 80.
- Connect to instance, select instance and click connect and then agian connect. you are connect to EC2 machine from browser shell ssh.

Networking

- Doing this on the AWS Console
  - Login to Console > EC2 > `Elastic IPs`
    - Allocate Elastic IP for EC2.
  - Search VPC > Wizard > Pub & Pvt `subnets`
    - Add VPC Name > Then add AZs to subnets. Here one zone, AZ-a is selected.
  - VPC > Subnets - you can add `more subnets` by
    - Create Subnet
    - Select VPC ID > Subnet Name > AZ-b > CIDR in range > create.
    - Similarly, create private too.
  - Now `Routing-Tables`
    - Click Route table
    - select route-table for VPC ID
    - click routes - see that internet traffic is goint to NAT Gateway.
      - click `subnet-associations` tab
    - Similarly do for public.
  - Create `Security Group` for more security
    - add name
    - add to vpc
    - inbound: http, source: anywhere. Outbound added auto.
  - Create `EC2` > configuration
    - Network: Lab VPC
    - Subnet: public 1
    - auto assign public IP: enable
    - Config security group, pick the one created
  - Create second EC2 similarly and add second public subnet.
- Now the app should work with added security, high-availability and resource restriction.


Storage Demonstration: Create an Amazon S3 Bucket

- S3 bucket is created in region, not tied to subnet
- Open Console > search S3 > Create `Bucket`
- Bucket Name: emp-photo-bucket-012
- Region: place in same region as of ec2
- create bucket
- Open Bucket
- Click `Permission` tab
- Bucket Policy > edit > enter new Policy JOSN, `IAM` role to allow app access to bucket.
- Add files as objects - click `upload`
- Bucket should be accessible via app.


Scaling

- demo
  - make `launch template`, what to launch when scaling
  - console - ec2 - sidebar launch template - create - give name and desc - check autoscaling
  - AMI - create mirror image of web server, select AMI AND t2 MICRO.
  - SELECT SAME KEY-PAIR, same security group, expand advance
  - IAM same role
  - pase user data.
  - This completes 'what to launch'
  - Now, 'when to launch'
    - sidebar - 'autoscaling group' - create - select template - same vpc, select public subnets
    - attach to load balancer.
    - define group size.


- demo: ec2 - sidebar - load balancers - create - ALB - give a name
  - scheme
    - internet facing - to manage client req
    - internal load balancer (pvt IP to pvt IP) - for 3 tier apps
  - listeners - default + HTTPs
  - availability zones - choose vpc, check both availability zone and public subnets
  - security group - all port 80 from anywhere
  - routing - give name - next - chosse instances - next - create.
  - find the DNS name in detais, open it.
  - this is app being server from two availablity zones and the traffic is managed by the load balancer.

Demonstration: Configure High Availability for Your Application

- Database Dynamo and file server S3 are both highly available and scalabel within a region. Only EC2 will limit. So we make it available by using load balancer and scalable using autoscaling, which adds and removes instances based on load.

Employee Directory Application Redesign

- The Architecture
  - the app is hosted across ec2 instances inside VPC, in private subnets.
  - ec2 part of autoscaling, traffic is managed by app load balancer
  - db on dynamo
  - file in s3
- to ensure this all works, analyse that auto scaling policy is working as expected. may need some tweaking to work over time. Also install security patched and updates for EC2 as they come out.
- Now, we can redisgn the app to make it completely serverless using Cloud Native service like AWS Lambda. and explore other architectures possible.
- As of now, the app is a 3 tier app
  - presentation Layer - UI - HTML, CSS, JS or Mobile
  - application Layer - Business/Application Logic
  - data layer - database
- EC2 is being used for both, presentation layer and application. To improve this we can separate, these layers.
- Presentation on S3, as static website. Not all sites are static, but JS can help with this, React sites are static and make the content dynamic by using JS to make HTTP requests.
- Application Layer to be hosted as Lambda, each of CRUD as separate Lambda function.
- Amazon API Gateway, can be used to make front end to talk to lambda functions serving different events in backend.
  - API on API Gateway acts as a front door to trigger the backend code on lambda. We can have one labda function for all events or separate for each.
- Dynamo for database, s3 for files.
- alla ccess handeled by RBAC via IAM.
- This way we can make the app modular. this can help make changes quickly without making whole infra to change and test. untouched DB, but can update code.
- Futher, Amazon RT53 can be used to manage domains name calls
- CloudFront can be used to catch static content and make it available close to end users using global infra of AWS
- With this serverless architecture, compared to EC2 solution, we have made the app scalable, available and thus can reduce cost. VPC and networking is msanged.
- Another option is using container services. All service in AWS is API based, thus we can use any of them

### DEMO - Build a Serverless Web Application

with AWS Lambda, Amazon API Gateway, AWS Amplify, Amazon DynamoDB, and Amazon Cognito

Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

![AWS Build a Serverless Web Application](https://d1.awsstatic.com/diagrams/Serverless_Architecture.5434f715486a0bdd5786cd1c084cd96efa82438f.png)

Static Web Hosting - AWS Amplify hosts static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser.

User Management - Amazon Cognito provides user management and authentication functions to secure the backend API.

Serverless Backend - Amazon DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

RESTful API - JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway.









## Links

- <https://explore.skillbuilder.aws/learn/course/1851/play/45289/aws-technical-essentials-104>
- More about compute, how to select between ec2, labda and container - 40 mins - <https://explore.skillbuilder.aws/learn/course/internal/view/elearning/199/aws-compute-services-overview?dt=tile&tile=fdt>
- Data Analytics, volume, variety, velocity, veracity ETL, value VIZ - 4hr - <https://explore.skillbuilder.aws/learn/course/internal/view/elearning/44/data-analytics-fundamentals?dt=tile&tile=fdt>
- `Heroku` is and alternative PaaS for deploying container-based apps on cloud.
- Build a Serverless Web Application
with AWS Lambda, Amazon API Gateway, AWS Amplify, Amazon DynamoDB, and Amazon Cognito. Links:
- <https://aws.amazon.com/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/>
- <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-dynamo-db.html>
- <https://medium.com/rahasak/build-serverless-application-with-aws-amplify-aws-api-gateway-aws-lambda-and-cognito-auth-a8606b9cb025>
- <https://trackit.io/aws-api-gateway-create-api-python-cognito-serverless/>

You don't necessarily need a static server in order to run a Create React App project in production.
Static web app allows you to host static pages written on frameworks such as Angular, react, vuejs, etc.


