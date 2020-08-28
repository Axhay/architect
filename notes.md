
# Table of Contents

[Spot Instances](#Spot-Instance)

[RedShift](#RedShift)

[Identity and Access Management (IAM)](#Identity-and-Access-Management-(IAM))

[Cloudfront OAI](#Cloudfront-OAI)

[Lambda Functions](#Lambda-Functions)

[Cloudwatch](#Cloudwatch)

[Simple Storage Service](#Simple-Storage-Service (S3))

[Amazon Athena](#Amazon-Athena)

[Virtual Private Cloud](#Virtual-Private-Cloud (VPC))

[Elastic Beanstalk](#Elastic-Beanstalk)

[Elastic Block Storage](#Elastic-Block-Storage (EBS))

[Elastic File Stoarge (EFS)](#Elastic-File-Storage)

[CloudFormation](#CloudFormation)

[Relational Database Service](#Relational-Database-Service)

[DynamoDB](#DynamoDB)

[NAT Gateway](#NAT-Gateway)

[EC2 Instance and Instance Types](#EC2-Instance-and-Instance-Types)

[Security Groups and ALB](#Security-Groups-and-ALB)

[CloudTrail](#CloudTrail)

[Kinesis](#Kinesis)

[API Gateway](#API-Gateway)

[Load Balancers](#Load-Balancers)

[ElasticCache](#Elastic-Cache)

[Simple Queuing Service](#Simple-Queuing-Service)

[Route 53](#Route-53)

[Messaging Queue](#Messaging-Queue)

[Network Loadbalancers](#Network-Loadbalancers)

[Miscellaneous](#Miscellaneous)

## Spot Instance

1. Recover from Instance Failures.
2. Continue processing from where you left off.
3. Process files in parallel.

## RedShift

1. Data Warehouse.
2. Does not scale automatically.
3. Partly resides in S3.
4. Run complex, analytic queries against petabytes of stuctured data.
5. Structured and unstructured.
6. SQL Queries
7. Petabyte Scale.
8. Enable data encryption for your clusters to protect data at rest.
9. Data Archival aggregated over last 12 months.
10. Resilient data warehouse solution.
11. Encrypt all data in the Redshift cluster at Rest using AWS KMS Default Customer master key.

## Identity and Access Management (IAM)

1. Assigning an role is giving an access.
2. IAM users cannnot be attached to EC2 instance but IAM role can be attached to EC2 instance.
3. For AWS service to service interaction without need to create credentials, we use IAM roles. In that case we define a policy list RDS instances, use it to create a role and attach the role to the lambda function.
4. For applications running outside of AWS such as on-premises and third-party automation, it will need access keys, NOT IAM Role

- Policy
  - Effect: Allow or Deny
  - Action: Include a list of actions that the policy allows or denies (effect)
  - Resource: a list of resources to which the actions apply

## Cloudfront OAI

1. CDN offered by AWS. 
2. CDN provides a globally distributed network of proxy servers with cache content, such as web videos or other bulky media, more locally to customers, thus improving access speed for downloading the content.
3. Website serves millions of users globally.
4. Minimize/Reduce data transfer cost.
5. More cost effective for more frequently accessed objects.
6. Host static website in S3 bucket.
7. Deliver your entire website or application including static, dynamic, streaming and interactive content.
8. Users located in many places across the world.
9. Cache option for cloudfront which cache static and dynamic content is the best option.
10. Location wise content delivery is already enabled.
11. Most cost effective than S3 transfer for frequently accessed objects.
12. Improve image load time.
13. No cost of data transfer between S3 and CloudFront.
14. Cloudfront can access bucket on behalf of requestors.
15. No cost of data transfer between S3 and cloudfront.
16. Cache option of cloud front in best solution.
17. OAI: Content is available for 14 days before user is denied access.
18. Allow users to purchase premium and shared content in S3 bucket.
19. Restrict access with least operational overhead.

## Lambda Functions

1. Improve fault tolerance of existing Python application.
2. Least operation overhead.
3. Serverless service.
4. AWS will manage provisioning and managing.
5. Redesign the process for high availability.
6. Lambda is triggered in 3 ways:
  a. API Gateway event.
  b. DynamoDB events.
  c. S3 events.

## Cloudwatch

1. Must include a two minute warning.
2. Memory -> only custom cloudwatch.
3. Cloudtrail logs Api calls.
4. You can configure Amazon CloudWatch Events to invoke a Lambda action in response to suspicious or unexpected behavior in your AWS environment detected by Amazon GuardDuty.
5. The unified CloudWatch agent enables you to do the following:
Collect more system-level metrics from Amazon EC2 instances across operating systems. The metrics can include in-guest metrics, in addition to the metrics for EC2 instances. The additional metrics that can be collected are listed in Metrics Collected by the CloudWatch Agent.
6. There are 2 main types of monitoring you can do on AWS EC2 Instances as follows
✑ Basic Monitoring for Amazon EC2 instances: Seven pre-selected metrics at five-minute frequency and three status check metrics at one-minute frequency, for no additional charge.
✑ Detailed Monitoring for Amazon EC2 instances: All metrics available to Basic Monitoring at one-minute frequency, for an additional charge. Instances with
Detailed Monitoring enabled allows data aggregation by Amazon EC2 AMI ID and instance type.

## Simple Storage Service (S3)

1. S3 can handle at least 3,500 PUT/COPY/PASTE/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.
2. S3 is storage solution.
3. Glacier is archive solution.
4. S3 is an object storage.

S3 Pre-signed URLS:

1. Prevent unauthorized access to the reports.
2. Temporary access to protected assets.
3. Simple, occassional sharing of private files.
4. Frequent programmatic access to view or upload a file in an application.
5. Thumbnail easily created from originals if they are accidentally deleted.

S3 standard storage:

1. Files accessed multiple times a day for first 30 days.

S3 Standard Infrequent access:

1. Files rarely accessed in next 90 days.
2. Documents must be retreived immediately.
3. Highly available access but not frequent access.
4. Immediate access for batch jobs.
5. Media companies have more than 100TB of data to be stored and retrieved infrequently.

Glacier storage:

1. Transition data to Glacier storage after 7 days.
2. Archived data: ex after 7 days, data can be archived.
3. Data is rarely accessed after 35 days.
4. Readily available means glacier storage.
5. Cost of Glacier is less than IA.
6. Customers can store data for as little as $1 per terabyte per month. Data stored in Glacier is immutable, meaning after an archive is created it cannot be updated.


## Amazon Athena

1. Company wants to minimize infrastructure.
2. Serverless service.
3. Does not need any infrastructure to create, manage of scale datasets.
4. Works directly on top of S3 data sets.
5. Creates external tables and therefore does not manipulate S3 data sources.

## Virtual Private Cloud

1. Data never leave AWS network - VPC endpoint.
2. Latest generation of VPC endpoints used by AWS Systems manager are powered by Private link.
3. Eggress only for IPv6.
4. AWS Privatelink provides private connectivity between VPCs, AWS services, and on-premises applications, securely on the Amazon network. PII
5. VPC flog logs is a feature that enable to capture information about IP traffic going to and from network interfaces in your VPC. 
6. VPC endpoint enables privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS private link without requiring an internet gateway, NAT device, VPN connection or AWS Direct Connect connection.
7. Flow logs is the only service that allows you to monitor the flow of Network IP addresses.
8. VPC peering is a communication within AWS accounts and internet access is not required.
9. You can specify allow rules but not deny rules.
10. A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.
11. If you host a website on multiple Amazon EC2 instances, you can distribute traffic to your website across the instances by using an Elastic Load Balancing (ELB) load balancer.

## Elastic Beanstalk

1. Automatically scales your app up and down based on your applications specific need using easily adjustable Auto scaling settings.
2. With Elastic Beanstalk, your application can handle peaks in workload or traffic while minimizing your costs.
3. Automatically handles deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring, you just have to concentrate on your code.
4. Elastic Beanstalk is a AWS managed service that can do scalability. No auto scaling required.

## Elastic Block Storage

1. Cold HDD (sc1):
- Throughput-oriented storage for large volumes of data that is infrequently accessed
- Scenarios where the lowest storage cost is important
- Cannot be a boot volume
2. very high sequential I/O and lowest cost
3. Provisioned IOPS. Keywords: consistent performance, database, High I/O
4. hdd : large, sequential i/o operations
ssd : small, random i/o operations
5. Because EBS is block storage while EC2 is Compute,S3 Object
6. Once you see OLTP (transaction processing DB), I/O intensive DB the most consistent EBS storage is provisioned IOPS
7. With Elastic Volumes, you can dynamically modify the size, performance, and volume type of your Amazon EBS volumes without detaching them.
Use the following process when modifying a volume:
(Optional) Before modifying a volume that contains valuable data, it is a best practice to create a snapshot of the volume in case you need to roll back your changes. For more information, see Creating Amazon EBS Snapshots.

## Elastic File Storage

1. EFS are as good as NFS on Linux
"migrating to AWS" and "without code changes" are the key words.
2. "Mount storage" keyword : EFS.
3. Key word : strong data consistency and file locking
4. File-locking = EFS
5. When you hear POSIX-compliant, first thought is normally AWS EFS:

Amazon EFS provides elastic, shared file storage that is POSIX-compliant. The file system you create supports concurrent read and write access from multiple Amazon EC2 instances and is accessible from all of the Availability Zones in the AWS Region where it is created.

## CloudFormation

1. Cloud formation is the IAC tool ( infrastructure as code)
2. Replicate the environment of on premises to AWS by using CloudFormation.
3. For Availability Multi AZ, For Performance Read Replica

## RDS - Relational Database Service

1. With RDS Storage Auto Scaling, you simply set your desired maximum storage limit, and Auto Scaling takes care of the rest.
2. You can only enable encryption for an Amazon RDS DB instance when you create it, not after the DB instance is created.
For an Amazon RDS encrypted DB instance, all logs, backups, and snapshots are encrypted.
3. DynamoDB is HA. But Postgre is'nt. So a Multi AZ is needed.
4. You can enable point-in-time recovery using the AWS Management Console, AWS Command Line Interface (AWS CLI), or the DynamoDB API. When it's enabled, point-in-time recovery provides continuous backups until you explicitly turn it off.
DR takes things to a completely new level, wherein you need to be able to recover from a different region that’s separated by over 250 miles.
5. Think read replicas when performance is mentioned. Think Multi-AZ when high availability is mentioned
6. In a Multi AZ, AWS runs just one DB but copies the data synchronously to the standby replica
The question targets Read Contention( responsiveness ) and write is not an issue and hence the Read Replicas.
7. The application servers and database must be secure. so it must be on a private subnet.
In question itself we have	 answer. ELB already in public subnet .Elb will communicate to private subnet.

## DynamoDB 

1. Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's a fully managed, multiregion, multimaster, durable database with built-in security, backup and restore, and in-memory caching for internet-scale applications. DynamoDB can handle more than 10 trillion requests per day and can support peaks of more than 20 million requests per second.
Many of the world's fastest growing businesses such as Lyft, Airbnb, and Redfin as well as enterprises such as Samsung, Toyota, and Capital One depend on the scale and performance of DynamoDB to support their mission-critical workloads.
Hundreds of thousands of AWS customers have chosen DynamoDB as their key-value and document database for mobile, web, gaming, ad tech, IoT, and other applications that need low-latency data access at any scale. Create a new table for your application and let DynamoDB handle the rest.
2. Extreme Performance While DynamoDB offers consistent single-digit millisecond latency, DynamoDB + DAX takes performance to the next level with response times in microseconds for millions of requests per second for read-heavy workloads. With DAX, your applications remain fast and responsive, even when a popular event or news story drives unprecedented request volumes your way. No tuning required.
3. DAX handles millions of req per sec
4. Let’s assume you want to take a backup of one of your DynamoDB tables each day. A simple way to achieve this is to use an Amazon CloudWatch Events rule to trigger an AWS Lambda function daily. In this scenario, in your Lambda function you have the code required to call the dynamodb:CreateBackup API operation
5. You can tell the application to perform a strongly consistent read. This will force DynamoDB to return the most up to date information.
6. AWS Data pipeline is the web service you can use to automate the movement and transformation of data.  You can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks. You define the parameters of your data transformations and AWS data Pipeline enforces the logic that you have setup.
7. Many companies consider migrating from relational databases like MySQL to Amazon DynamoDB, a fully managed, fast, highly scalable, and flexible NoSQL database service. For example, DynamoDB can increase or decrease capacity based on traffic, in accordance with business needs. The total cost of servicing can be optimized more easily than for the typical media-based RDBMS.
8. The 8KB is significant as it fits inside of the 400KB item size limit of DynamoDB and there is viable:
Items
Item Size
The maximum item size in DynamoDB is 400 KB
Other than this low latency retrieval, durable and 'cost is not a factor' all points to the DynamoDB answer here.
9. DynamoDB is typically useful for storing a large number of small records with single digit millisecond latency.
10. DynamoDB. Keywords: indexed data
11. "key = value" and "semi-structured" ==> NoSQL DB = DynamoDB
12. Amazon DynamoDB transactions simplify the developer experience of making coordinated, all-or-nothing changes to multiple items both within and across tables. Transactions provide atomicity, consistency, isolation, and durability (ACID) in DynamoDB, enabling you to maintain data correctness in your applications easily.
13. Requirements: MOST cost-efficient change to support the increased traffic with minimal changes to the application
* Cost effective: only scale up when needed
* No change to application

## EC2 Instance and Instance Types

1. "multicontainer-based" => EC2, path-base routing: ALB
2. Default Termination Policy
This section describes the default termination policy used by an Auto Scaling group when a scale-in event occurs. The default termination policy is designed to help ensure that your instances span Availability Zones evenly for high availability. The default policy is kept generic and flexible to cover a range of scenarios.
With the default termination policy, the behavior of the Auto Scaling group is as follows:

1) Determine which Availability Zone(s) have the most instances, and at least one instance that is not protected from scale in.
If there are multiple unprotected instances to choose from in the Availability Zone(s) with the most instances, an instance is selected for termination based on the following criteria (applied in the order shown).
3. Scaling based on a schedule enables you to scale your application in response to predictable changes in demand. To use scheduled scaling, you create scheduled actions, which tell Spot Fleet to perform scaling activities at specific times. When you create a scheduled action, you specify the Spot Fleet, when the scaling activity should occur, minimum capacity, and maximum capacity. You can create scheduled actions that scale one time only or that scale on a recurring schedule.
4. Sec groups are stateful and only accept Allow rules.
5. Security Group: Traffic can be restricted by any IP protocol, by service port, and source/destination IP address (individual IP or CIDR block).
6. Predictive Scaling, a feature of AWS Auto Scaling uses machine learning to schedule the right number of EC2 instances in anticipation of approaching traffic changes. Predictive Scaling predicts future traffic, including regularly-occurring spikes, and provisions the right number of EC2 instances in advance.

  ## Security Groups and ALB

  1. Application Load Balancers have no IP address so using the security group for it is the best fit in this case.
  2. Unlike a traditional web hosting model, inbound network traffic filtering should not be confined to the edge; it should also be applied at the host level. Amazon EC2 provides a feature named security groups. A security group is analogous to an inbound network firewall, for which you can specify the protocols, ports, and source
  IP ranges that are allowed to reach your EC2 instances. You can assign one or more security groups to each EC2 instance. Each security group routes the appropriate traffic to each instance. Security groups can be configured so that only specific subnets or IP addresses have access to an EC2 instance. Or they can reference other security groups to limit access to EC2 instances that are in specific groups.
  3. ALB doesn't have an IP Address.
  4. Using Redis AUTH command can improve data security by requiring the user to enter a password before they are granted permission to execute Redis commands on a password-protected Redis server
  5. Security Groups only allow you to 'Allow' traffic, not 'Deny' it (making Answer B not possible)
  https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html
   Security Group Basics
   The following are the basic characteristics of security groups for your VPC:
   You can specify allow rules, but not deny rules.
  
## CloudTrail

1. Once you enable Cloudtrail on a root account, it will log all API interactions on the account and it will also propagate automatically to any new region defined in the account.


## Kinesis

1. Kinesis=event=1000 request
2. Amazon Kinesis Data Firehose is the easiest way to reliably load streaming data into data lakes, data stores, and analytics tools. ... It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration.
  
## API Gateway

1. Lambda and API because it has to scale based on demand. SPOT instances do work for stateless apps, should be in a comparable price versus lambda but the issue is elasticity and how these instances are going to be available (or the lack of certainty they will).
2. Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. With a few clicks in the AWS Management Console, you can create REST and WebSocket APIs that act as a “front door” for applications to access data, business logic, or functionality from your backend services, such as workloads running on Amazon Elastic Compute Cloud (Amazon EC2), code running on AWS Lambda, any web application, or real-time communication applications.
API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management. API Gateway has no minimum fees or startup costs.
  
## NAT Gateway

1. Note If you have resources in multiple Availability Zones and they share one NAT gateway, in the event that the NAT gateway’s Availability Zone is down, resources in the other Availability Zones lose internet access. To create an Availability Zone-independent architecture, create a NAT gateway in each Availability Zone and configure your routing to ensure that resources use the NAT gateway in the same Availability Zone. 
2. Connect to the Internet using Network Address Translation (private subnets) – Private subnets can be used for instances that you do not want to be directly addressable from the Internet. Instances in a private subnet can access the Internet without exposing their private IP address by routing their traffic through a Network Address Translation (NAT) gateway in a public subnet.
3. Reason, The web Servers in web subnet can get Internet via ELB or IGW but private cannot go through ELB and IGW. They either need Nat Instance or Nat Gateway. since NAT instances are not mentioned then it must be Nat Gateway.
4. you must always create your NAT gateway in a public subnet that already has a defined route to 0.0.0.0/0 through an internet gateway. and assign an Elastic IP to the NAT gateway.
5. You have a limit on the number of NAT gateways you can create in an Availability Zone. 

## Load Balancers

1. An application load balancer can take the place of multiple individual classic load balancers to load balance microservices (e.g. running on AWS ECS) saving costs.
Most importantly though, COST. Application Load Balancers cost $0.0252 per hour. Classic Load Balancers cost $0.028 per hour.                           A great article is here to refer: https://medium.com/cognitoiq/how-cognitoiq-are-using-application-load-balancers-to-cut-elastic-load-balancing-cost-by-90-78d4e980624b

## ElasticCache

1. Session data - Elasticache.
2. Enabling ElastiCache Multi-AZ with automatic failover on your Redis cluster (in the API and CLI, replication group) improves your fault tolerance. This is true particularly in cases where your cluster's read/write primary cluster becomes unreachable or fails for any reason. Multi-AZ with automatic failover is only supported on Redis clusters that support replication
3. Scheduled Instances are a good choice for workloads that do not run continuously, but do run on a regular schedule. For example, you can use Scheduled Instances for an application that runs during business hours or for batch processing that runs at the end of the week.
If you require a capacity reservation on a continuous basis, Reserved Instances might meet your needs and decrease costs.

## SQS

1. FIFO will deliver atleast once and no duplicates.
2. Default queue could deliver twice can have duplicates.
3. Decoupling this element and having it processed by separate workers allows the rest of the system to not suffer the impact of the slow-running worker.
https://medium.com/@pablo.iorio/synchronous-and-asynchronous-aws-decoupling-solutions-1bbe74697db9

## Route 53

1. MultiValue Answer Routing would fit to random queries with health checks.
2. You can use the cloudfront with Route 53 Geolocation Routing. But the location wise content delivery is already enabled in cloudfront, so geolocation policy wont help that much. If you are not using cloudfront and you want to distribute traffic based on user location, then you can use Route53
3. For an active-active system, Geolocation, Geoproximity, Latency, Multivalue, or Weighted policies would work.
https://medium.com/dazn-tech/how-to-implement-the-perfect-failover-strategy-using-amazon-route53-1cc4b19fa9c7

## MQ

1. Amazon MQ is a managed message broker service for Apache ActiveMQ that makes it easy to set up and operate message brokers in the cloud. Message brokers allow different software systems–often using different programming languages, and on different platforms–to communicate and exchange information. With Amazon MQ, you can use industry standard APIs and protocols for messaging, including JMS, NMS, AMQP, STOMP, MQTT, and WebSocket. You can easily move from any message broker that uses these standards to Amazon MQ because you don’t have to rewrite any messaging code in your applications.

## Network Load Balancer

1. Choose an Application Load Balancer when you need a flexible feature set for your web applications with HTTP and HTTPS traffic.
Choose a Network Load Balancer when you need ultra-high performance, TLS offloading at scale, centralized certificate deployment, support for UDP, and static IP addresses for your application.
2. Host-based routing use host conditions to define rules that forward requests to different target groups based on the host name in the host header. This enables ALB to support multiple domains using a single load balancer.
Path-based routing use path conditions to define rules that forward requests to different target groups based on the URL in the request. Each path condition has one path pattern. If the URL in a request matches the path pattern in a listener rule exactly, the request is routed using that rule.
3. The Health check is configured for ELB and failing, however the Auto Scaling needs to use the ELB health check, in addition to the system checks, to determine if the instance in healthy. Hence the Auto Scaling group needs to be updated to use ELB health check for terminating the instances.
4. Application Load Balancer. Keywords: Distribute based on URL = Path based routing for ALB
5. For example, previously Amazon S3 performance guidelines recommended randomizing prefix naming with hashed characters to optimize performance for frequent data retrieval. You no longer have to randomize prefix naming for performance, and can use sequential date-based naming for your prefixes.
