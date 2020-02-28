
# Table of Contents

[Spot Instances](#Spot-Instance)

[RedShift](#RedShift)

[Storage Gateway](#Storage-Gateway)

[Identity and Access Management (IAM)](#Identity-and-Access-Management-(IAM))

[Amazon Aurora](#Amazon-Aurora)

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
12. Encrypt all the data in Redshift cluster.
  - Encrypt data at rest?
     - **Use AWS KMS Default customer master key.**
13. **Redshift spectrum**:
  - Run queries directly against Exabytes of structured and unstructured data in S3 without need for unnecessary data movement.

- Scenarios where you use Redshift:

1. Company has two types of reporting needs on their 200GB data warehouse.
    - Data scientists runs small concurrent ad hoc SQL queries takes several minutes to run.
    - Display screens may run fast Sql queries to populate dashboards.

2. Business teams need storage solution to store companies historical sales data.
   - 4TB of data will grow to hundreds of terabytes in few years.
   - Fast performance needed despite data set growth.
   - Run queries against current BI tools.
 
3. Company wants to analyze all sales information over last 12 months.
   - Company expects there to be over 10TB of data from multiple sources.
   - Useful for data archival.

## Storage Gateway

- Scenario where we can use this.
1. Legacy app need to interact with local storage using iSCSI.
   - The team needs to design a reliable storage solution to provision all new storage on AWS.
   - Which storage sol meets the legacy app requirements?
      - **AWS Storage gateway in cached mode for the legacy application storage to write data to S3.**

## Identity and Access Management (IAM)

1. Assigning an role is giving an access.
2. IAM users cannnot be attached to EC2 instance but IAM role can be attached to EC2 instance.
3. For AWS service to service interaction without need to create credentials, we use IAM roles. In that case we define a policy list RDS instances, use it to create a role and attach the role to the lambda function.
4. For applications running outside of AWS such as on-premises and third-party automation, it will need access keys, NOT IAM Role

Scenarios:

1. Create a solution where user access to multiple MySQL database is securely managed with short lived connection details.
   - To meet requirements:
      - **Create a user account to use AWS-Provided AWSAuthenticationPlugin with IAM.**

2. Company hosts a website using API gateway on the front end.
   - There is heavy traffic on website.
   - Company wnats to control access by allowing authenticated traffic only.
   - How should company limit access to authenticated users only?
      - **Allow users that are authenticated using Amazon Cognito.**
      - **Assign permissions in AWS IAM to allow users.**

3. SA is designing a new workload where AWS Lambda function will access an DynamoDB table.
   - What is most secure means to granting Lambda function access to DynamoDB table?
      - **Create an identity and access management (IAM) role with necessary permissions to access DynamoDB table, and assign the role to Lambda function.**

4. Company is creating a web application that will run on an EC2 instance.
   - Application the instance needs access to DynamoDB table for storage.
   - To meet requirement:
      - **Create IAM role and assign role to the EC2 instance with permissions to the DynamoDB table.**

5. SA is developing a software on AWS that requires access to multiple AWS services, including EC2 instance/
   - This is a security sensitive app, and AWS credentials such as Access Key ID and Secret Access Key need to be protected and cannot be exposed anywhere in the system.
   - To meet this security requirement:
      - **Assign IAM role to the EC2 instance.**

6. SA is designing  a web app that is running on EC2 instance.
   - App stores data in DynamoDB.
   - SA needs to secure access to DynamoDB table.
   - To achieve secure authorization:
      - **Create an IAM role with permissions to write to DynamoDB table.**
      - **Attach an IAM role to the EC2 instance.**

7. SA is designing a lambda function that calls an API to list all running RDS instances. How is request authorized?
      - **Create IAM role to the Lambda function with permissions to list all Amazon RDS instances.**

8. App runs on EC2 instance must make secure calls to S3 bucket.
    - How to ensure calls are made without exposing credentials?
      - **Create an IAM role granting least privilege and assign it to EC2 instance profile.**

9. Legacy app running in premises require SA to be able to open a firewall to allow access to several S3 buckets.
   - Architect has a VPN connection to AWS in place.
   - To meet this requirement:
      - **Create an IAM role that allows access from corporate network to S3.**


- Policy

  - Effect: Allow or Deny

  - Action: Include a list of actions that the policy allows or denies (effect)

  - Resource: a list of resources to which the actions apply

  - Principle:

    - The `Principal` element specifies the user, account, service, or other entity that is allowed or denied access to a resource

    - Mostly for resource-based policy, which attached to services such as SQS, S3, KMS. These services have their own policy management system with same language as IAM


## Amazon Aurora

Scenarios:

1. Bank is writing new software dependant on DB transcations for write consistency.
   - App will do joins across multiple tables.
   - DB must automatically scale as the amount of data grows.
   - AWS service to run the database.
     - **Amazon Aurora.**

2. Design Disaster recovery (DR) in separate AWS region from applications primary workload.
   - Multi tier architecture and only RDS instance have frequent changes.
   - App installation takes 60 mins on average.
   - DR have an RPO of less than 90 mins and RTO of less than 30 mins. 
   - To meet these:
     - **An Aurora instance as primary database with read replica in DR region.**
     - **A cross region EC2 AMI copy.**

3. App stores data in RDS MySQL DB instance.
   - DB queries consists of read queries, which are overwhelming current database.
   - SA wants to scale the database.
   - What combination of steps to achieve goal?
     - **Migrate MySQL database to Aurora.**
     - **Create read replicas in different AZ.**

4. Team launching marketing campaign and the peak database read activity in Aurora for MySQL is expected to increase.
   - SA decides to add two Read replicas to the cluster.
   - How should SA ensure that connections for read activities are load balanced?
      - **Reader endpoint for amazon Aurora.**

5. SA is creating new relational DB.
   - Compliance team will use DB, and mandates that data content must be stored across three different AZ.
   - Which option to use?
      - **Aurora.**

6. Company wants to migrate a highly transactional DB to AWS.
   - Requirements state that the DB has more than 6TB of data and will grow exponentially.
   - Which solution should SA recommend?
      - **Amazon Aurora.**

7. Web app stores all data in RDS Aurora DB instance.
   - SA wants to provide access to the data for detailed report for marketing team, but is concerned that additional load on the DB will affect the performance of web app.
   - How can report be created without affecting performance of application?
      - **Create a read replica of the database.**

8. Popular e-commerse app runs on AWS.
   - The app encounters performance issues.
   - DB is unable to handle amount of queries and load during peak hours.
   - The DB is running on RDS Aurora engine on the largest instance size available.
   - What should admin do to improve performance?
      - **Create one or more read replicas.**


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

Scenarios:

1. Org uses S3 to store video content via the website.
    - Org has right to deliver this content to users within its own country and needs to restrict access.
    - Ensure files are accessible only from within the country.

2. Company runs a static website served through cloudfront.
    - Storing website content in S3 instead of EBS.
       - **S3 is an origin for cloud front.**
       - **EBS volumes need EC2 instances behind an ELB load balancer to be an origin.**

3. Website runs on EC2 instances behind ELB ALB.
    - Instances run in EC2 auto scaling group across multiple AZ.
    - Every night, auto scaling group doubles in size.
    - Traffic shows that users in particular region requesting same static content stored locally on EC2 instances.
    - How can SA reduce need to scale and improve app performance for users.
        - **Create cloudfront distribution for the site and redirect user traffic to the distribution.**

4. Retail company runs on EC2 instances behind ALB. Instances run in EC2 auto scaling group.

    - Images are stored in S3 bucket using custom domain name.
    - During flash sale with 10,000 simultaneous users, some images on website are not loading.
    - How to improve performance?
       - **Config cloud front with S3 bucket as origin.**

5. SA needs to design a solution that allows web devs to deploy static web content without managing server infrastructure.

    - All web content must be accessed over HTTPS without custom domain name.
    - Sol be scalable as company grows.
    - Which provides most cost effective solution?
      - **Cloudfront with S3 bucket origin.**

6. Company needs to expand web services from us-east-1 to ap-southeast-1.
    - Company stores large amount of static content on its web and recently received complaints about slow loading speeds and website timing out.
    - What can be done to meet expansion goal and also addressing latency and timeout issues?
      - **Use S3 to store static content and configure Amazon Cloudfront distribution.**

7. Company creating web app allows customers to view photos in their web browsers.
    - Web is hosted in us-east-1 on EC2 instances behind an ALB.
    - Users will be located in many places around the world.
    - Which sol provides all users with fastest photo viewing experience.
       - **Enable cloudfront for website and specify ALB as the origin.**

8. Company has application that uses Cloudfront for content hosted on S3 bucket.
    - After unexpected refresh, users are still seeing old content.
    - Which step SA take to ensure new content is displayed?
       - **Change TTL value for removing the old objects.**

9. Company has website on premises.
    - Website has mix of static and dynamic content, but users experience latency when loading static files.
    - Which AWS service can help reduce latency?
       - **CloudFront with onpremises servers as the origin.**

10. Reduce compute costs due to serving high amounts of static web content.
    - How can web server architecture be designed to be MOST cost efficient?
       - **Create CloudFront distribution to pull static content from S3 bucket.**

11. Company using S3 bucket in us-west-2 to serve videos to their customers.
    - Customers are located all around the world and videos are requested a lot during peak hours.
    - Customers in Europe complain about slow downloaded speeds, and during peak hours, customers in all locations report experiencing HTTP 500 errors. 
    - What can SA do to address these issues?
       - **Cache the web content with CloudFront and use all Edge locations for content delivery.**

12. Ability to handle higher volumes. Web tier is cost-optimized.
       - **Launch EC2 instances in an Auto scaling group behind an ELB.**
       - **Create an CloudFront distribution pointing to static content in S3.**

13. Company is launching new static web on S3 and cloud front.
       - **Ensure all request goes through cloudfront.**

14. SA is asked to deliver video content on S3 to specific users from cloudfront restricting access by unauthorized users.
       - **Use OAI for cloudfront to access private S3 objects and select Restruct Viewer access option in Cloudfront cache behavior to use Signed URLs.**

15. Company wants to improve latency by hosting images in S3 bucket.
  - Need to restrict access to S3 bucket allowing cloud front to continue proper functionality.
  - Making bucket private to restrict access with LEAST operational overhead.
       - **Update the bucket policy to grant access to it.**
       - **Create CloudFront origin access identity.**

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

Scenarios:

1. Prediction process needs access to trained models stored on S3 bucket.
  - Few seconds to process the image and make predictions.
  - Process not overly resource intensive.
  - Does not need specialized hardware.
  - Takes less than 512MB to run.
  - Most cost effective solution:
       - **Lambda Functions**

2. Lambda function needs access to RDS for SQL server instance.
  - It is against the company policy to store passwords in lambda functions.
  - How can SA enable lambda functions to retreive the DB password without violating company policy?
       - **Use AWS Systems Manager Parameter Store**

## Cloudwatch

1. Must include a two minute warning.
2. Memory -> only custom cloudwatch.
3. Cloudtrail logs Api calls.
4. You can configure Amazon CloudWatch Events to invoke a Lambda action in response to suspicious or unexpected behavior in your AWS environment detected by Amazon GuardDuty.
5. The unified CloudWatch agent enables you to do the following:
Collect more system-level metrics from Amazon EC2 instances across operating systems. The metrics can include in-guest metrics, in addition to the metrics for EC2 instances. The additional metrics that can be collected are listed in Metrics Collected by the CloudWatch Agent.

Collect system-level metrics from on-premises servers. These can include servers in a hybrid environment as well as servers not managed by AWS.
✑ Retrieve custom metrics from your applications or services using the StatsD and collectd protocols. StatsD is supported on both Linux servers and servers running Windows Server. collectd is supported only on Linux servers.
✑ Collect logs from Amazon EC2 instances and on-premises servers, running either Linux or Windows Server.
You can store and view the metrics that you collect with the CloudWatch agent in CloudWatch just as you can with any other CloudWatch metrics. The default namespace for metrics collected by the CloudWatch agent is CWAgent, although you can specify a different namespace when you configure the agent.
6. There are 2 main types of monitoring you can do on AWS EC2 Instances as follows
✑ Basic Monitoring for Amazon EC2 instances: Seven pre-selected metrics at five-minute frequency and three status check metrics at one-minute frequency, for no additional charge.
✑ Detailed Monitoring for Amazon EC2 instances: All metrics available to Basic Monitoring at one-minute frequency, for an additional charge. Instances with
Detailed Monitoring enabled allows data aggregation by Amazon EC2 AMI ID and instance type.
7. 

Scenarios:

1. Ecommerce application places orders in SQS queue.
  - When a message is received, EC2 worker instances process the request.
  - EC2 instances are in Auto Scaling group.
  - How should architecture be designed to scale up and down with LEAST amount of operational overhead?
       - **Use an Amazon Cloudwatch alarm based on the number of visible messages to scale the Auto Scaling group up or down.**

2. SA needs to configure scaling policied based on Cloudwatch metrics for an Auto scaling group.
  - The application running on the instances is memory intensive.
  - How can Architect meet this requirement?
       - **Publish custom metrics to CloudWatch from the application.**

3. SA is designing a solution to send CloudWatch Alarm notifications to a group of users on a smartphone mobile application.
  - What are key steps to this solution?
        - **Configure cloudwatch alarm to send the notification to an Amazon SNS topic whenever there is an alarm.**
        - **Create the platform endpoints for mobile devices and subscribe the SNS topic with platform endpoints.**

4. Web App runs on 10 EC2 instances launched from single customer Amazon Machine Language (AMI).
  - EC2 instances are behind an Interet application load balancer.
  - Amazon Route 53 provides DNS for the app.
  - How should a SA automate recovery when a web server instance stops replying to requests?
       - **Add cloudwatch alarm actions for each instance to restart if the Status check(Any) fails.**

5. SA must migrate monolithic onpremises application to AWS.
  - It is a web app with a load balancer, web server, application server, and relational database.
  - Key requirement driving the migration is that app should perform better and be more elastic.
  - Which of the architecture would meet these requirements?
       - **Replatform the application as three tier application. Use Elastic load balancing for incoming requests. Use EC2 for web and application tiers. Use CloudWatch alarms and Auto Scaling for horizontal scaling at the web tier.**

6. App runs on EC2 instances in an Auto Scaling group.
  - When instances are terminated, the Systems operations team cannot determine the root cause, because the logs on the terminated instances are lost.
  - How can root cause be determined?
       - **Use Amazon Cloudwatch agent to push the logs to Amazon Cloudwatch logs.**

7. Org has long running image processing app that runs on Spot instances that will be terminated when interrupted. 
  - A highly available workload must be designed to respond to Spot Instance interruption notices. 
  - The solution must include a two minute warning when there is not enough capacity. 
  - How can these requirements be met?
       - **Use Amazon CloudWatch Events to invoke an AWS Lambda function that can launch On-Demand Instances.**

8. SA is designing a solution that can monitor memory and disk space utilization of all EC2 instances running Amazon Linux and Windows.
  - Which solution meets this requirement?
        - **Custom Amazon CloudWatch metrics**

9. SA notices slower response times from an application.
  - CloudWatch metrics on the MySQL RDS indicate Read IOPS are high and fluctuate significantly when the database is under load.
  - How should the DB environment be re-designed to resolve the IOPS fluctuation?
       - **Change the storage type to Provisioned IOPS.**

10. SA is designing sol that includes a managed VPN connection.
  - To monitor whether the VPN connection is up or down, architect should use:
       - **the CloudWatch TunnelState Metric.**

11. Company wants to use Amazon GuardDuty to detect malicious and unexpected activity.
  - They want to use CloudWatch to ensure that when findings occur, remediation occurs automatically.
  - Which cloudwatch feature you use to trigger AWS Lambda function to perform remediation?
       - **Events**

12. Design a new web app on Amazon EC2. 
  - System must make application specific metrics, such as application security events, available to the sysops team.
  - How do SA enable this in the design?
       - **Install the Amazon CloudWatch Logs agent on the application instances. Design the application to store events in application log files.**

13. Application calls service run by vendor. Vendor charges on the number of calls. Finance department needs to know number of calls that are made to the service to validate the billing statements.
  - How can SA design a system to durably store number of calls without requiring changes to the application?
       - **Publish a custom Amazon CloudWatch metric that counts calls to the service.**

14. S3 data stored with encryption at rest:
  - Implementing data lake solution on S3
  - Data lake has two types of encryption:  Server side and Client side
  - Data stored in S3 must be encrypted at rest. Which option can achieve this?
        - **Use S3 server-side encryption with customer-provided keys (SSE-C).**
        - **Use client-side encryption before ingesting the data to Amazon S3 using encryption keys.**

15. Encrypt data in S3.
  -  Encryption keys generated and managed on-premises. How can SA meet security requirements?
        - **SSE-C: Server-side encryption with customer-provided encryption keys**

## Simple Storage Service (S3)

1. S3 can handle at least 3,500 PUT/COPY/PASTE/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.
2. S3 is storage solution.
3. Glacier is archive solution.
4. S3 is an object storage.
5. 


Scenarios:

1. SA is architecting a workload that requires a performant object based storage system that must be shared with multiple EC2 instances.
  - Company expects its user base to increase five times over one year.
  - Its application is hosted in one region and users RDS MySQL DB, an ELB ALB, and ECS to host the website and its micro services.
  - Which design changes should SA recommend to support expected growth?
        -**- Move static files from ECS to Amazon S3.**

2. Company is evaluating S3 as data storage solution for their daily analyst reports.
  - Company has implemented stringent requirements concerning the security of the data at rest.
  - Specifically, CISO asked for the use of envelope encryption with separate permissions  for the use of an envelope key, automated rotation of the encryption keys and visibility into when an encryption key was used and by whom.
  - Which steps should SA take to satisfy the security requirements requested by CISO?
       - **Create S3 bucket to store reports and use Server-side encryption with AWS KMS-Managed keys (SSE-KMS).**

S3 Pre-signed URLS:

1. Prevent unauthorized access to the reports.
2. Temporary access to protected assets.
3. Simple, occassional sharing of private files.
4. Frequent programmatic access to view or upload a file in an application.
5. Thumbnail easily created from originals if they are accidentally deleted.

Scenarios:

3. App provides users to download private and personal files. 
  - Web server is overwhelmed with serving files for download.
  - It should reduce web server load and allow users to download only their files.
       - **Store files in S3 and have app generate S3 pre-signed URL for the user to download.**

4. Photo sharing website allows to generate thumbnail images of photo stored in S3.
  - DynamoDB table maintains location of photos and thumbnail are easily created from originals if accidentally deleted.
  - Storing thumbnails images at lowest cost
        - **S3**

5. Thousands of files stored in S3 bucket has well defined access pattern.
  - Files accessed multiple times a day for first 30 days.
  - Files rarely accessed in 90 days.
  - After that files never accessed again.
  - During first 120 days accessing files needs to take no more than few seconds.
  - Which lifecycle policy to minimize cost based on access pattern?
        - **S3 standard storage for first 30 days.Then move files to S3 Standard IA for next 90 days. Allow files to expire after that.**

6. SA is designing new social media application.
  - App must provide secure method of uploading profile photos.
  - Each user should be able to upload a photo into shared storage location for one week after their profile is created.
  - Which approach will meet all of these requirements?
       - **Use Amazon S3 with default private access policy and generate pre-signed URLs each time a new site profile is created.**

S3 standard storage:

1. Files accessed multiple times a day for first 30 days.

Scenario:

7. Creating serverless web app
  - Access mapping data in hundreds of data files, each containing 30 KB of data.
  - Storage expected to grow to hundreds of terabytes.
  - Most cost effective storage.
       - **S3 standard storage**

8. SA is building online shopping app where users be able to browse items, add items and purchase items.
  - Images of items stored in S3 buckets organized by item category.
  - During testing, item images are still available to some users on deletion.
  - What is the flaw in design?
        - **S3 Delete requests are eventually consistent, which may cause other users to view items that have already been purchased.**

S3 Standard Infrequent access:

1. Files rarely accessed in next 90 days.
2. Documents must be retreived immediately.
3. Highly available access but not frequent access.
4. Immediate access for batch jobs.
5. Media companies have more than 100TB of data to be stored and retrieved infrequently.

Scenarios:

9. Company uses S3 for Backup of on premises environment.
  - Data must be retained at least for 7 years.
  - Data infrequently accessed for 35 days but needs to be instantly available.
  - After 35 days, data is rarely accessed
  - Most cost effective solution.
       - **Change backup so data goes to S3 Standard IA directly**
       - **Creates an S3 lifecycle policy that moves data to GLACIER storage class after 35 days.**

10. Company wants to store data for 5 years.
  - Company need to have immediate and highly available access to data in any point of time but not require frequent access.
  - What lifecycle action should be taken to meet requirements while reducing costs?
        - **Transition objects from S3 standard to S3 standard-IA.**

11. Company designs new service receiving locations updates from 3,600 rental cars every hour.
  - Cars upload location to S3 bucket.
  - Each location must be checked for distance from original rental location.
  - Which service process updates and scale automatically?
        - **S3 events and AWS lambda**

12. SA is designing a system that will store personally identifiable information (PII) to S3.
  - Due to compliance and regulatory requirements, both the master keys and unencrypted data should never be sent to AWS.
  - Which S3 encryption should SA choose?
       - **S3 client-side encryption with client-side master key**

13. Customer has service based out of Oregon, us and Paris, France.
  - App is storing data in S3 in Oregon and data is updated frequently.
  - Paris is experiencing slow response time when retrieving objects.
  - What should SA do to resolve slow response time for Paris office?
        - **Set up S3 bucket in Paris, and enable cross-region replication from Oregon bucket to Paris bucket.**

14. Company uses S3 for storing variety of files.
  - SA needs to design a feature that allow users to instantly restore any deleted files within 30 days of deletion.
  - Which is most cost efficient solution?
       - **Enable versioning and create lifecycle policy to remove expired versions after 30 days.**

15. One company wants to share contents of S3 bucket with other company.
  - sec mandate that only other companies AWS account has access to contents of S3 bucket.
  - Which feature allow secure access to S3 bucket.
       - **Bucket policy**

16. Company policy requires that all data stored in S3 is encrypted.
  - Company wants to use option of least overhead and do not want to manage any encryption keys.
  - Which option meets companys requirement?
       - **Server Side Encryption (SSE-S3)**

17. Company wants to ensure that all files created in S3 bucket us-east-1 are also available in another bucket in ap-southeast-2.
  - Which option represents the simplest way to implement these changes?
        - **Enable versioning and configure cross-region replication from bucket in us-east-1 to bucket in ap-southeast-2.**

18. SA is architecting a workload that needs performant object based storage system that must be shared with multiple EC2 instances.
  - Which AWS service meets these requirements?
       - **Amazon S3.**

19. Company has an app that store sensitive data. 
  - Companies are required to store multiple copies of its data.
  - What would be MOST resilient and cost effective option to meet this requirement?
       - **S3**

20. SA is designing solution to store large quantities of event data in S3.
  - Architect anticipates that workload will exceed 100 requests each second.
  - What should SA do in S3 to optimize requirements?
       - **Randomize a key name prefix.**

21. Companies dev teams plans to create S3 bucket containing millions of images.
  - Team want to maximize read performance of S3.
  - Which naming scheme company should use?
       - **Add a date as the prefix.**

22. Company storing app data in S3 buckets across multiple AWS regions.
  - Company policy requires that encryption keys be generated at company headquarters, but encryption keys be stored in AWS after generation.
  - SA plans to configure cross region replication.
  - Which sol will encrypt data requiring least amount of operational overhead?
       - **Configure S3 buckets to use Server-side encryption with AWS KMS-Managed Keys (SSE-KMS) with imported key material in both regions.**

23. SA must design a storage solution for incoming billing reports in CSV format.
  - Data does not need to be scanned frequently and is discarded after 30 days.
  - Which service will be MOST cost-effective in meeting these requirements?
       - **Write the files to an S3 bucket and use Amazon Athena to query the data.**

24. S3 bucket have the policy to delete the files automatically after 30 day retention period has elapsed further saving money.
  - Company uses S3  for storing variety of files.
  - SA needs to  design feature that will allow users to instantly restore any deleted files within 30 days of deletion. 
  - Which is the most cost efficient solution ?
       - **Enable versioning and create a lifecycle policy to remove expired versions after 30 days.**

25. Company generates invoices and makes it available online.
  - Stored as PDF in S3 bucket.
  - Customers see invoice during start of the month.
  - Past invoice needs to be immediately available.
  - Concerns over rising storage costs as company gains more customers.
  - Most cost effective method to store data?
       - **S3 for current invoices**
       - **Setup lifecycle rules to migrate invoices to S3 Standard IA after 30 days.**

26. App produces monthly reports that must be immediately accessible up to 7 days.
  - After 7 days, data can be archived.
  - Archived data be retrievable within 24 hrs of request.
  - Most cost effective approach to satisfy compliance requirement?
        - **Store data in S3 standard storage with lifecycle rule to transition the data to GLACIER storage after 7 days.**

Glacier storage:

1. Transition data to Glacier storage after 7 days.
2. Archived data: ex after 7 days, data can be archived.
3. Data is rarely accessed after 35 days.
4. Readily available means glacier storage.
5. Cost of Glacier is less than IA.
6. Customers can store data for as little as $1 per terabyte per month. Data stored in Glacier is immutable, meaning after an archive is created it cannot be updated.


Scenario:

27. App generates audit logs of operational activities
  - Compliance requirements mandates that the app retains logs for 5 years.
  - How can requirements be met?
       - **Save the logs in an amazon glacier vault and use the vault lock feature.**

28. Company processed 10TB of raw data to generate quarterly reports.
  - Unlikely to be used again, raw data needs to be preserved for compliance and auditing purpose.
  - Most cost effective way to store data in aws?
        - **Glacier**

29. Media company store 10TB of audio recordings:
  - Retreival happens infrequently and requestors agree on an 8-hour turnaround time.
  - What is the MOST cost effective solution to store files?
       - **Amazon Glacier**

30. SA is asked to improve fault tolerance of existing python application.
  - The web application places 1MB images in S3 bucket.
  - App then uses single T2.large instance to transform image to include watermark with companys brand before writing the image back to S3 bucket.
  - What should SA recommend to increase fault tolerance of the solution?
        - **Convert the code to lambda function triggered by S3 events.**

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

Scenarios:

1. App server should be in private subnet without access to the Internet.
  - Sol must retrieve and upload files to S3 bucket.
        - **Use Amazon S3 VPC endpoints.**

2. App running in private subnet accesses DynamoDB table.
  - There is security requirement that data never leaves AWS network.
  - How is this requirement met?
        - **Create a VPC endpoint for DynamoDB and configure the endpoint policy**

3. SA creating an application running on VPC that needs to access AWS Systems Manager Parameter Store.
  - Network security rules prohibit route table entry with 0.0.0.0./0 destination
  - Which infrastructure allows access to AWS service meeting requirements?
       - **AWS PrivateLink**

4. Design VPC that requires access to remote API server using IPv6.
  - Resources within the VPC should not be accessed directly from Internet.
  - How can this be achieved?
       - **Attach an egress-only internet gateway and update the routing tables.**

5. Company wants to create app that transmit protected health information to thousands of service consumers in different AWS accounts.
  - App servers will sit in private VPC subnets.
  - Routing for the app must be fault tolerant.
  - What to do to meet this requirement?
       - **Create VPC endpoint service and grant permissions to specific service consumers to create a connection.**

6. Security team reviewed their companys VPC flow logs and found that traffic is being directed to the internet.
  - App in the VPC uses EC2 instances for compute and S3 for storage.
  - Companys goal is to eliminate internet access and allow app to continue to function.
  - What change should be made in VPC before  updating route table?
       - **Create a VPC endpoint for Amazon S3 access.**

7. Most secure way to allow applications to access service endpoints in the same region.
       - **Use AWS PrivateLink**

8. Credit card processing app hosted on an on-premises server, needs to communicate directly with the database hosted on EC2 instance running in an private subnet of vpc.

  - Compliance req state that end to end communication should be encrypted.
  - Which solution will ensure that this requirement is met?
        - **Use HTTPS for traffic over the VPN connection between the VPC and the on-premises datacenter.**
        - **Communication are encrypted and secured simply by using VPN.**

9. SA is given following requirements for a companys VPC:
    - Two tiered app with web and db tier
    - All web traffic to environment should be directed from internet to ALB
    - Web servers and DB should not obtain public IP address or subnet with any other device.
    - Environment be highly available within same VPC for all services.
    - What is the min number of subnets that SA need for best practices?
        - **6**
        - 2 public subnets for ELB
        - 2 private subnets for web server.
        - 2 private subnets for databases.

10. Company plans to use VPC to deploy web app consisting of ELB, a fleet of web and app servers, and RDS MySQL db that should not be accessible from the Internet.
  - Proposed design must be highly available and distributed over two AZ.
  - What is most  appropriate VPC design for this use case?
       - **Two public subnet for ELB, two private subnets for web servers and two private subnets for RDS.**

11. SA is designing an app on AWS that will connect to on-premise data center through VPN connection.
  - Sol must be able to log network traffic over VPN. 
  - Which service logs this network traffic?
       - **Amazon VPC flow logs**

12. SA is designing VPC.
  - App in VPC must have private connectivity to DynamoDB in the same AWS region.
  - Design should route DynamoDB traffic through
       - **VPC endpoint**

13. Company requires that source, destination and protocol of all IP packets be recorded when traversing a private subnet.
  -  What is the MOST secure and reliable method of accomplishing this goal?
       - **Create VPC flow logs on the subnet.**

14. SA is designing a new app that needs to access data in different AWS account located in the same region.
  - The data must not be accessed over the Internet.
  - Which solution will meet these requirements with LOWEST cost?
       - **Establish a VPC peering connection between accounts.**

15. SA is designing network architecture for an app that has compliance requirements.
  - App will be hosted on EC2 instances in private subnet and will be using S3 for storing data.
  - Compliance requirements mandate that the data cannot traverse the public internet.
  - What is the MOST secure way to satisfy this requirement?
       - **Use a VPC endpoint.**

16. Dev team is building an app with front end and back end app tiers.
  - Each tier consists of EC2 instances behind an ELB CLB.
  - Instances run in Auto scaling group across multi AZ.
  - Network team has allocated the 10.0.0.0/24 address space for this app.
  - Only the front end load balancer should be exposed to the Internet.
  - There are concerns about limited size of address space and ability of each tier to scale.
  - What should the VPC subnet design be in each AZ? 
       - **One public subnet for the load balancer tier, one public subnet for the front-end tier, and one private subnet for the backend tier.**

17. SA is designing VPC. Applications in VPC must have private connectivity to DynamoDB in the same AWS region. The design should route DynamoDB traffic through:
       - **VPC endpoint**

18. Company hosts a popular web app. The web app connects to DB running in private VPC subnet. The web servers must be accessible only to customers on an SSL connection. RDS MySQL DB server must be accessible only from web servers. How should SA design a solution to meet requirements without impacting running apps?
        - **Open an HTTPS port on the security group for web servers and set the source to 0.0.0.0/0. Open the MySQL port on the database security group and attach it to the MySQL instance. Set the source to Web Server Security Group.**

19. A workload in an VPC consists of a single web tier server launched from a custom AMI. Session state is stored in a DB. How should a SA modify this workload to be both highly available and scalable?
        - **Create a launch configuration with the AMI ID of the web server image. Create an Auto Scaling group using the newly-created launch configuration, and a desired capacity of two web servers across multiple Availability Zones. Use an ALB to balance traffic across the Auto Scaling group.**

## Elastic Beanstalk

1. Automatically scales your app up and down based on your applications specific need using easily adjustable Auto scaling settings.
2. With Elastic Beanstalk, your application can handle peaks in workload or traffic while minimizing your costs.
3. Automatically handles deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring, you just have to concentrate on your code.
4. Elastic Beanstalk is a AWS managed service that can do scalability. No auto scaling required.

Scenarios:

1. During review of Business applications, SA identifies critical application with relational database that was built by business user and is running on users desktop.
  - To reduce the risk of business interruption, SA wants to migrate application to highly available, multi-tiered solution in AWS.
  - How can SA accomplish this with least disruption to business?
       - **Use AWS DMS to migrate the backend database to an Amazon RDS Multi-AZ DB instance. Migrate the application code to AWS Elastic Beanstalk**

2. Company has web app running in docker container that connects to a MySQL server in an on-premises data center. 
  - Deployment and maintenance of this app is becoming time consuming and slowing down new feature releases.
  - The company wants to migrate app to AWS and use services that helps facilitate infrastructure management and deployment.
  - Which architectures should company consider on AWS?
       - **Amazon ECS for the web application, and an Amazon RDS for MySQL for the database.**
       - **AWS Elastic Beanstalk Docker Single Container for the web application, and an Amazon RDS for MySQL for the database.**

3. Company uses Elastic Beanstalk to deploy a web app running on c4.large instances. 
  - Users are reporting high latency and failed requests.
  - Further investigation reveals that the EC2 instances are running at or near 100% CPU utilization.
  - What should SA do to address the performance issues?
       - **Modify the scaling triggers in Elastic Beanstalk to use the CPUUtilization metric.**

4. SA needs to deploy a node.js based web application that is highly available and scales automatically.
  - Marketing team needs to rollback on app releases quickly, and they need to have an operational dashboard.
  - The Marketing team does not want to manage deployment of OS patches to the linux servers.
  - Which AWS service will satisfy these requirements?
        - **AWS Elastic Beanstalk**

5. SA is developing a new web app on AWS
  - The services must scale to support an increasing load.
  - The architect wants to focus on software development and deploying new features rather than provisioning or managing servers.
  - Which AWS service is appropriate?
        - **Elastic Beanstalk**

6. Which service should org use if it requires an easily managed and scalable platform to host its web app running on Nginx?
       - **AWS Elastic Beanstalk**

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


Scenarios:

1. Customer has legacy application with large amount of data.
  - Files accessed by app are around 10GB each, but rarely accessed.
  - When accessed they are retrieved sequentially.
  - Customer is migrating the app to AWS and would like to use EC2 and EBS.
  - Least expensive EBS volume type in this use case?
       - **Cold HDD (sc1)**

2. Consider possible options for improving security of the data on EBS volume attached to EC2 instance. 
  - Which solution will improve the security of the data?
       - **Use AWS KMS to encrypt the EBS volume**

3. Company deployed three tier web app on EBS backed EC2 instances for web and app tiers, and RDS for database tier.
  - Company is concerned about the loss of data on the web and app tiers.
  - What is the most efficient way to prevent data loss?
       - **Create a snapshot lifecycle policy that takes periodic snapshots of the Amazon EBS volumes**

4. Company has many applications on EC2 instances running in auto scaling groups.
  - Company policies require that data on EBS volume must be retained.
  - Which actions will meet these requirements without impacting performance?
        - **Disable DeleteOnTermination for the Amazon EBS volumes.**

5. SA is trying to bring data warehouse workload to an EC2 instance. 
  - The data will reside in EBS volumes and full table scans will be executed frequently. 
  - What type of EBS volume would be most suitable in this scenario?
        - **Throughput Optimized HDD (st1)**

6. SA must select storage type for big data app that requires very high sequential I/O.
  - The data must persist if the instance is stopped.
  - Which of the following storage types will provide best fit at the LOWEST cost for the app?
        - **An Amazon EBS throughput optimized HDD volume.**

7. AWS workload in a VPC is running a legacy database on EC2 instance. 
 - Data is stored on 200GB EBS (gp2) volume. 
 - At peak load times, logs show excessive wait time. 
 - What solution should be implemented to improve DB performance using persistent storage?
       - **Migrate the data on the EBS volume to provisioned IOPS SSD (io1).**

8. Company is migrating its data center to AWS.
  - As part of this migration, there is a three-tier web app that has strict data-at-rest encryption requirements. 
  - Customer deploys this app on EC2 using EBS and now must provide encryption at-rest. 
  - How can these requirements be met without changing the application?
       - **Use encrypted EBS storage volumes with AWS-managed keys.**

9. SA is designing a DB solution that must support a high rate of random disk reads and writes.
 - It must provide consistent performance, and requires long term persistence. 
 - Which storage solution BEST meets these requirements
       - **An Amazon EBS Provisioned IOPS volume**

10. SA is designing a solution for a media company that will stream large amounts of data from EC2 instance. 
 - Data streams are large and sequential, and must be able to support up to 500MB/s. 
 - Which storage type will meet performance requirements of this app?
        - **EBS Throughput Optimized HDD**

11. SA is designing an app on AWS that uses persistent block storage.
 - Data must be encrypted at rest. 
 - Which sol meets the requirement?
       - **Encrypt Amazon EBS volumes on Amazon EC2 instances.**

12. Company has a legacy app using a proprietary file system and plans to migrate the app to AWS.
 - Which storage service should the company use?
       - **Amazon EBS**

13. SA is designing a log-processing solution that requires storage that supports up to 500MB/s throughput.
 - The data is sequentially accessed by EC2 instance.
 - Which storage type satisfies these requirements?
       - **EBS Throughput Optimized HDD (st1)**

14. Devs are creating a new OLTP app for a small DB that is very read-write intensive. 
 - A single table in the database is updated continuously throughout the day, and the devs want to ensure that the DB performance is consistent.
 - Which EBS storage option will achieve the MOST consistent performance to help maintain app performance?
       - **Provisioned IOPS SSD**

15. SA is designing an app that uses EBS volumes.
 - The volumes must be backed up to a different region. How should SA meet these requirements?
       - **Create EBS snapshots and then copy them to the desired region.**

16. An app requires block storage for file updates.
 - Data is 500 GB and must continuously sustain 100MB/s of aggregate read/write operations.
 - Which storage option is appropriate for this app?
       - **Amazon EBS**

17. SA must review an application deployed on EC2 instances that currently stores multiple 5-GB files on attached instance store volumes.
 - The company recently experienced a significant data loss after stopping and starting their instances and wants to prevent the data loss from happening again.
 - Sol should minimize performance impact and the number of code changes required.
 - What should SA recommend?
        - **Store the application data in an EBS volume**

18. Which requirement must be met in order for SA to specify that an EC2 instance should stop rather than terminate when its Spot instance is interrupted?
        - **The Spot Instance request type must be persistent.**
        - **The root volume must be an Amazon EBS volume.**

19. Company runs a legacy app with single tier architecture on EC2 instance.
 - Disk I/O is low, with occasional small spikes during business hours.
 - Company requires the instance to be stopped from 8PM to 8AM daily.
 - Which storage option is MOST appropriate for this workload?
       - **Amazon EBS General Purpose SSD (gp2) storage**

20. SA is designing the storage layer for a production relational DB.
 - DB will run on EC2. DB is accessed by an application that performs intensive reads and writes, so the DB requires the LOWEST random I/O latency.
 - Which data storage method fulfills the above requirements?
       - **Stripe data across multiple Amazon EBS volumes using RAID 0.**

Keyword "Hosted on EC2 and we know EC2 uses block storage =EBS. Plus RAID 0 increases performance

21. SA is building an app on AWS that will need 20,000 IOPS on particular volume to support media event. 
 - Once the event ends, the IOPS need is no longer required.
 - Marketing team asks the Architect to build a platform to optimize storage without incurring downtime. 
 - How should SA design a platform to meet these requirements?
       - **Change the EBS volume type to Provisioned IOPS.**

22. Company is writing a new service running on EC2 that must create thumbnail images of thousands of images in a large archive. The system will write scratch data to storage during the process. Which storage service is best suited for this scenario?
       - **Amazon EBS Throughput Optimized HDD (st1)**

## Elastic File Storage

1. EFS are as good as NFS on Linux
"migrating to AWS" and "without code changes" are the key words.
2. "Mount storage" keyword : EFS.
3. Key word : strong data consistency and file locking
4. File-locking = EFS
5. When you hear POSIX-compliant, first thought is normally AWS EFS:

Amazon EFS provides elastic, shared file storage that is POSIX-compliant. The file system you create supports concurrent read and write access from multiple Amazon EC2 instances and is accessible from all of the Availability Zones in the AWS Region where it is created.

Scenarios:

1. Customer is migrating to AWS and requires applications to access Network File System shares without code changes.
 - Data is critical and accessed frequently.
 - Which storage solution should SA recommend to maximize availability and duration?
        - **Amazon EFS**

2. Design elastic application that will have between 10 to 50 EC2 concurrent instances running, dependent on load.
 - Each instance must mount storage that will read and write to the same 50GB folder.
 - What storage type will meet requirements?
       - **Amazon EFS**

3. Company is deploying a reporting application on EC2.
 - Application is expected to generate 1,000 documents every hour and each document will be 800 MB.
 - Company is concerned about strong data consistency and file locking, as various applications hosted on other EC2 instances will process report documents in parallel when they become available.
 - What storage solution will meet these requirements with LEAST amount of administrative overhead?
        - **Amazon EFS**

4. SA is designing a shared file system for a company.
 -  Multiple users will be accessing it at any given time.
 - Different teams will have their own directories, and company wants to secure files so that users can access only files owned by their team.
 - How should SA design this?
       - **Use Amazon EFS and control permissions by using file-level permissions.**

5. SA is helping customers to migrate applications to AWS.
 - The application is composed of a fleet of Linux servers that currently use shared file system to read and write data. 
 - One of the goals of moving this application to AWS is to increase the reliability of the storage tier.
 - What solution would increase reliability while minimizing the operational overhead of managing this infrastructure?
       - **Create an EFS file system and mount it to all the servers.**

6. An application runs on multiple EC2 instances.
 - Each running instance of the app must have access to a shared file system.
 - Where should the data be stored?
       - **Amazon EFS**

7. SA is developing a solution for sharing files in an organization.
 -  Solution must allow multiple users to access the storage service at once from different virtual machines and scales automatically. 
 - It must also support file-level locking.
 - Which storage service meets the requirements of this use case?
       - **Amazon EFS**

8. Media company asked SA to design a highly available storage sol to serve as a centralized document store for their EC2 instances.
 - The storage sol needs to be POSIX-compliant, scale dynamically and be able to serve up to 100 concurrent EC2 instances. 
 - Which solution meets these requirements?
       - **Create an Amazon Elastic File System (Amazon EFS) to store and share the documents.**

## CloudFormation

1. Cloud formation is the IAC tool ( infrastructure as code)
2. Replicate the environment of on premises to AWS by using CloudFormation.
3. For Availability Multi AZ, For Performance Read Replica

Scenarios:
1. Clients notice that their engineers often make mistakes when creating SQS queues for their backend system.
 - Which action should SA recommend to improve this process?
       - **Use AWS CloudFormation Templates to manage the Amazon SQS queue creation.**

2. SA is designing a new app that will  be hosted on EC2 instances.
 - This app has the following traffic requirements.
 - Accept HTTP(80)/HTTPS(443) traffic from the internet. 
 -  Accept FTP(21) traffic from the finance team servers at 10.10.2.0/24.
 -  Which of the following AWS cloudformation snippets correctly declares inbound security group rules that meet requirements and prevent unauthorized access to additional services on the instance?

       **IpProtocol:"udp""FromPort":"443""ToPort":"443""CidrIp":"0.0.0.0/0"**
       **IpProtocol:"udp""FromPort":"80""ToPort":"80""CidrIp":"0.0.0.0/0"**
       **IpProtocol:"udp""FromPort":"21""ToPort":"21""CidrIp":"10.10.2.0/24"**

TCP/80 and TCP/443 from any

3. SA has designed a VPC that meets all necessary security requirements for their organization. 
 - Any applications deployed in the organization must use this VPC design.
 - How can project teams deploy, manage and delete VPCs that meet this design with LEAST admin effort?
        - **Deploy an AWS CloudFormation template that defines components of the VPC.**

4. SA was tasked with reviewing several templates that build VPCs and ensuring that they meet specific security requirements.
 - After reviewing the templates, the architect realizes that all of the templates are missing important security best practices.
 - How should SA do to implement security best practices in efficient manner?
       - **Provide the teams a nested AWS CloudFormation template that builds the VPC correctly**

5. SA needs to use AWS to implement pilot light disaster recovery for a three-tier web app hosted in an on-premises datacenter.
 - Which sol allows rapid provision of working, fully scaled production environment?
       - **Continuously replicate the production database server to Amazon RDS. Use AWS CloudFormation to deploy the application and any additional servers if necessary.**

## RDS - Relational Database Service.

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

Scenarios:

1. Customer deploying production portal application on AWS.
 - The database tier has structured data.
 - Company needs a solution that is easily manageable and highly available. 
       - **Use Amazon RDS with a multiple Availability Zone option.**

2. Retail company runs hourly flash sales has performance issue on RDS for PostgreSQL database.
 - DBA identified the issue with performance happens when finance and marketing employees refresh sales dashboards used for reporting real-time sales data. 
 - How to resolve issue without impacting performance?
       - **Create a Read Replica of the RDS PostgreSQL database and point the dashboards at the Read Replica.**

3. Companies Amazon RDS MySQL DB instance may be rebooted for maintenance and to apply patches.
 - This database is critical and potential user disruption must be minimized. 
 - What should SA do in this scenario?
       - **Set RDS MySQL to Multi-AZ.**

4. Company setting new website for online sales.
 - Company will have a web and database tier.
 - Web tier consists of load-balanced, auto-scaled EC2 instance in multiple AZ.
 - Database tier is an RDS Multi AZ deployment. 
 - EC2 instances must connect securely to DB.
 - How should resources be launched?
       - **EC2 instances: private subnet RDS database instances: private subnet Load balancer: public subnet**
ELB can be in public subnet while EC2 and RDS can be in private subnet

5. Company migrating on-prem DB to AWS
 -  Companys backend application produces large amount of DB queries for reporting purposes, and company wants to offload some of those reads to Read Replica, allowing primary database to continue performing efficiently.
 - Which AWS DB platforms will accomplish this?
        - **Amazon RDS for PostgreSQL**
        - **Amazon RDS for MariaDB**

6. What conditions will cause Multi-AZ RDS failover to occur?
        - **An Availability Zone becomes unavailable**
        - **A failure of the primary database instance**

7. During performance testing of an application, RDS database caused a performance bottleneck.
 - What steps can be taken to improve database performance?
       - **Scale up to a larger RDS instance type.**
       - **Redirect read queries to RDS read replicas.**

8. App running on EC2 has been experiencing performance issues when accessing RDS for Oracle database.
  - The database has been provisioned correctly for average workloads, but there are several usage spikes each day that have saturated the database, causing the application to time out.
  - The application is write-heavy, updating information more often than reading information. 
  - SA has been asked to review the application design.
  - What should the SA recommend to improve performance?
       - **Change the Amazon RDS instance storage type from General Purpose SSD to provisioned IOPS SSD.**
  
9. SA is designing a two tier application for maximum security, with a web tier running on EC2 instances and the data stored in an RDS DB instance.
 - The web tier should accept user access only through HTTPS connections (443) from the Internet, and data must be encrypted in transit to and from DB.
 - What combination of steps will MOST securely meet the stated requirements?
       - **Create a security group for the web tier instances that allows inbound traffic only over port 443.**
       - **Configure the web servers to communicate with RDS by using SSL, and issue certificates to the web tier EC2 instances.**

10. Company has an application that accesses MySQL database installed on a single EC2 instance. 
 - The instance recently experienced a fault and brought down the entire application for several hours.
 - Company wants to address the issue but is concerned about spending too much time modifying application code or managing the legacy application.
 - What should SA recommend to remove this single point of failure with FEWEST changes to the application code and the LEAST amount of admin effort?
       - **Migrate the database to an RDS MySQL Multi-AZ DB instance, and point the application servers to the new RDS instance.**

11. App uses an Amazon RDS MySQL cluster for the DB layer. 
 - DB growth requires periodic resizing of the instance. 
 - Currently, admins check the available disk space manually once a week. 
 - How can this process be improved?
       - **Use Auto Scaling to increase storage size.**

12. SA is designing a solution that will include database in RDS.
 - Corporate security policy mandates that the database, its logs, and its backups are all encrypted - Which is the MOST efficient option to fulfill the security policy using RDS?
        - **Launch an Amazon RDS instance with encryption enabled. Logs and backups are automatically encrypted.**

13. Team has an application that detects new objects being uploaded into S3 bucket. 
 - The uploads trigger a Lambda function to write object metadata into DynamoDB table and RDS PostgreSQL database. 
 - Which action should the team take to ensure high availability?
       - **Enable multi-AZ on the RDS PostgreSQL database.**

14. On-premises database is experiencing significant performance problems when running SQL queries. 
 - With 10 users, the lookups are performing as expected. 
 - As the number of users increases, the lookups take three times longer than expected to return values to an application.
 - Which action should SA take to maintain performance as the user count increases?
        - **Configure Amazon RDS with additional read replicas.**

15. Company has a legal requirement to store point-in time copies of its RDS PostGRESQL db instance in facilities that are at least 200 miles apart. 
 -  Use of which of the following provides the easiest way to comply with this requirement?
       - **Cross-region snapshot copy**

16. Company has RDS DB backing its production website.
 - The sales team needs to run queries against the DB to track training program effectiveness. 
 - Queries against the production database cannot impact performance, and the solution must be easy to maintain.
 - How can these requirements be met?
       - **Use an Amazon RDS read replica of the production database and allow the team to query against it.**

17. Client is migrating legacy web app to AWS cloud. The current system uses an Oracle DB as RDBMS. 
 - Backups occur every night, and the data is stored on-premises.
 - SA must automate the backups and identify a storage solution while keeping costs low.
 - Which AWS service will meet these requirements?
       - **Amazon RDS**

18. SA is deploying a new production MySQL DB on AWS.
 - It is critical that the database is highly available.
 - What should SA do to achieve this goal with RDS?
       - **Enable multi-AZ to create a standby database in a different Availability Zone.**
  
19. Company has RDS managed online transaction processing system that has very heavy read and write. 
 - The SA notices throughput issues with the system.
 - How can the responsiveness of the primary database be improved?
       - **Offload SELECT queries that can tolerate stale data to READ replica.**

20. App stack includes an ELB in public subnet, a fleet of EC2 instances in an auto scaling group, and RDS MySQL cluster. 
 - Users connect to the app from the Internet. 
 - App servers and DB must be secure.
 - How should SA perform this task?
       - **Create a private subnet for the Amazon EC2 instances and a private subnet for the Amazon RDS cluster.**

21. Customer has a production app that frequently overwrites and deletes data, the app requires the most up to date version of the data every time it is requested.
 - Which storage should SA recommend to best accomodate this use case? 
       - **Amazon RDS**

22. Lambda function must execute query against an RDS database in a private subnet.
 - Which steps are required to allow Lambda function to access the RDS database? 
       - **Create the Lambda function within the Amazon RDS VPC.**
       - **Change the ingress rules of the Amazon RDS security group, allowing the Lambda security group.**

23. SA is designing an architecture for mobile gaming app. App is expected to be very popular. 
 - The Architect needs to prevent the RDS MySQL db from becoming a bottleneck due to frequently accessed queries.
 - Which service or feature should the architect add to prevent a bottleneck?
       - **Amazon ElastiCache in front of the RDS MySQL Database**

24. App stores data in RDS PostgreSQL Multi-AZ database instance. Ratio of read to write requests is about 2 to 1. Recent increases in traffic are causing very high latency. How can this problem be solved?
       - **Create a read replica and send all read traffic to it.**

25. Customer is running a critical payroll system in Production environment in one data center and a DR environment in another. App includes load-balanced web servers and failover for MySQL database. Customers DR process is manual and error-prone. For this reason, management has asked IT to migration application to AWS and make it highly available so that IT no longer has to manually fail over the environment. How should SA migrate system to AWS?
       - **Migrate the production environment to span multiple Availability Zones, using Elastic Load Balancing and Multi-AZ Amazon RDS. Decommission the DR environment because it is no longer needed.**

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

Scenario:

1. Design ride sharing application. 
 - App needs consistent and single digit millisecond latency.
 - App must integrate with highly scalable and fully managed database service to track GPS coordinate and user data for all rides.
 - Database service to meet these requirements?
       - **Amazon DynamoDB**

2. Review application that writes data to an Amazon DynamoDB table on daily basis. 
 - Random table read occurs many times per second. 
 - Company wants to allow thousands of low-latency reads and avoid any negative impact to the rest of the application. 
 - What should SA do to meet companys goals
       - **Use DynamoDB Accelerator to cache reads.**

3. Three tiered applications are created to host small news articles.
 - App is expected to serve millions of users.
 - When breaking news occurs, site must handle very large spikes in traffic without impacting database performance. 
 - What design meets these requirements while minimizing costs? 
       - **Use Amazon DynamoDB Accelerator (DAX) to cache read operations to the database**
  
4. Company storing data in DynamoDB table and needs to take daily backups and retain them for 6 months. 
 - How can SA meet these requirements without impacting production workload?
        - **Use Amazon CloudWatch Events to trigger an AWS Lambda function that makes an on-demand backup of the table**

5. Company plans to deploy new application in AWS that reads and writes information to database.
 - Company wants to deploy the application in two different AWS regions in an active active configuration.
 - Database need to replicate to keep information in sync. 
 - What should be used to meet these requirements?
       - **Amazon DynamoDB with global tables**

6. App is scanning an Amazon DynamoDB table that was created with default settings.
 - App occasionally reads stale data when it queries the table. How can this issue be solved?
        - **Update the application to use strongly consistent reads.**

7. SA must design a DynamoDB table to store data about customer activities. 
 - The data is used to analyze recent customer behavior, so data that is less than a week old is heavily accessed and older data is accessed infrequently.
 - Data that is more than one month old never needs to be referenced by the application, but needs to be archived for year end analytics.
 - What is the most cost efficient way to meet these requirements?
       - **Create separate tables for each week's data with higher throughput for the current week.**
       - **Export the old table data from DynamoDB to Amazon S3 using AWS Data Pipeline, and delete the old table.**

8. Company has a Node.js application running on EC2 that currently retrieves data for customers from DynamoDB table.
 - Company is seeing many repeat queries for the same items, and the number of queries is continuing to increase as the application gains popularity.
 - What solution will reduce the number of read capacity units (RCUs) required while minimizing the amount of refactoring that must be done to the application?
        - **Use Amazon DynamoDB Accelerator (DAX) to provide a caching layer**

9. Company is rolling out a new web service but is unsure how many customers service will attract.
 - However, the company is unwilling to accept any downtime.
 - What could SA recommend to the company in order to keep track of customers current session data?
        - **Amazon DynamoDB**

10. University is running an internal web app on AWS that students can access from university n/w to check their exam results. The web app runs on EC2 instances and pulls results from DynamoDB table. Auto scaling is currently configured to add a new web server when CPU is greater than 80% for 5 mins. DynamoDB is config to increased both read and write capacity units by 5 when utilization is greater than 80% Exam results are released at 9am each monday, and 80% of students, attempt to access their unique results within first 30 mins. Despite auto scaling being enabled, students are complaining of slow response times and errors when they view the site. There are no performance complaints after 930am on monday. Which rec should SA make to improve performance in a cost-effective manner?
       - **Use a scheduled job to scale out EC2 before 9:00 a.m. on Monday and to scale down after 9:30 a.m.**

11. Company is looking for fully managed solution to store its players state info for rapidly growing game. App runs on multiple EC2 node, which can scale according to incoming traffic. Request can be routed to any of the nodes, therefore, state info must be stored in centralized DB. The players state info needs to be read with strong consistency and needs conditional updates for any changes. Which service would be MOST cost-effective and scale seamlessly?
       - **Amazon DynamoDB**

12. Customer owns MySQL DB that is accessed by various clients who expect at most, 100ms latency on requests. Once a record is stored in the DB, it rarely changed. Clients only access one record at a time. Database access has been increasing due to increased client demand. The resultant load will soon exceed capacity of most expensive hardware available for purchase. The customer wants to migrate to AWS, and is willing to change DB systems. Which service would alleviate db load issue and offer virtually unlimited scalability for the future?
       - **Amazon DynamoDB**

13. Company wants to durably store data in 8KB chunks. The company will access the data once every few months. However, when the company does access the data, it must be done with as little latency as possible. Which AWS service should SA recommend if cost is NOT a factor?
       - **Amazon DynamoDB**

14. SA is designing a solution to monitor weather changes by the minute. The frontend application is hosted on EC2 instance. The backend must be scalable to a virtually unlimited size, and data retrieval must occur with minimal latency. Which AWS service should the architect use to store the data and achieve these requirements?
        - **Amazon DynamoDB**

15. SA is building a new feature using Lambda to create metadata when a user uploads pic to S3. All metadata must be indexed. Which AWS service should SA use to store this metadata?
       - **Amazon DynamoDB**

16. Org is currently hosting a large amount of frequently accessed data consisting of key-value pairs and semi-structured documents in their data center. They are planning to move this data to AWS. Which one of the following services MOST effectively meets their needs?
       - **Amazon DynamoDB**

17. Company is launching an app that it expects to be very popular. The company needs a DB that can scale with the rest of the app. The schema will change frequently. The app cannot afford any downtime for DB changes. Which AWS service allows the company to achieve these objectives?
       - **Amazon DynamoDB**

18. Data analytics startup company asks SA to recommend AWS data store options for indexed data. Data processing engine will generate and input more than 64 TB of processed data every day, with item sizes reaching up to 300 KB. The startup is flexible with data storage and is more interested in DB that requires minimal effort to scale with growing dataset size. Which AWS data store service should SA recommend?
       - **Amazon DynamoDB**

19. Web app running on EC2 instances writes data synchronously to DynamoDB table configured for 60 write capacity units. During normal operation the app writes 50Kb/s to the tale, but can scale up to 500kb/s during peak hours. The app is currently throttling errors from DynamoDB table during peak hours. What is the MOST cost-efficient change to support increased traffic with min changes to app?
       - **Configure Amazon DynamoDB Auto Scaling to handle the extra demand.**

## NAT Gateway

1. Application servers deployed in private subnet need to integrate with third party service via internet. Changes to provide outbound internet connectivity in VPC without inbound internet connectivity to application servers.
       - **Create a NAT Gateway without attaching an Internet Gateway to the VPC.**

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

Scenarios:

1. Team has developed a new web application in AWS region that has three availability zones: AZ-a, AZ-b and AZ-c. Application need to be fault tolerant and needs at least six EC2 instances running at all times. Application must tolerate loss of connectivity to any AZ so that application can continue to run.
       - **AZ-a with six EC2 instances, AZ-b with six EC2 instances, and AZ-c with no EC2 instances.**
       - **AZ-a with three EC2 instances, AZ-b with three EC2 instances, and AZ-c with three EC2 instances.**

2. Design multi-container based web application. Part of the web application, /order and /sale-event must scale independently while maintianing single fully qualified domain name. Which AWS service will help architect build this platform?
       - **Amazon ELB Application Load Balancer**
       - **Amazon EC2 Container Service**

3. Environment has auto scaling group across two AZ referred to as AZ-a and AZ-b and default termination policy. AZ-a has four Amazon EC2 instances and AZ-b has three EC2 instances. No instance is protected from scale in. How will Auto Scaling proceed if there is a scale-in event?
       - **Auto Scaling selects the Availability Zone with four EC2 instances and then continues to evaluate.**

4. Company’s new web application running on EC2 across multiple AZ will be heavily accessed during regular business hours. After business hours, usage will be minimal. What fleet-scaling approach should be used to size the EC2 fleet to handle traffic demands?
       - **Scheduled scaling**

5. SA is designing an application that requires having six amazon EC2 instances running at all times. Application will be deployed in the sa-east-1 region which has three AZ. Which action will provide 100% fault tolerance and lowest cost in even that one AZ in region becomes unavailable?
       - **Deploy three Amazon EC2 instances in sa-east-1a, three Amazon EC2 instances in sa-east-1b, and three Amazon EC2 instances in sa-east-1c**

if one AZ goes down, 6 instances are still running

6. Company is launching dynamic website and the Operations team expects up to 10 times the traffic on launch date. Website is hosted on EC2 instances and traffic is distributed by Amazon Route 53. SA must ensure that there is enough backend capacity to meet users demand. The operation team wants to scale down as quick as possible after launch. What is the most cost effective and fault tolerant solution that will meet company's needs?
       - **Set up an Application Load Balancer to distribute traffic to multiple EC2 instances**
       - **Set up an Auto Scaling group across multiple Availability Zones for the website, and create scale-out and scale-in policies**

7. SA is designing new architecture that will use EC2 Auto Scaling group. Which of the following factors determine the health check grace period?
       - **How much of the application code is embedded in the AMI.**
       - **How long the bootstrap script takes to run.**

8. How can a user track memory usage in EC2 instance?
       - **Place an agent on the EC2 instance to push memory usage to an Amazon CloudWatch custom metric.**
       Cloudwatch + Memory = Agent + Custom Metric

9. Company running series of national TV campaigns. These 30-second ad will introduce sudden traffic peaks targeted at Node.js application. Company expects traffic to increase from five requests each minute to more than 5,000 requests each minute. Which AWS service SA uses to ensure traffic surges can be handled?
       - **An Auto Scaling group for EC2 instances**

10. Customer setup VPC with one private subnet and one public subnet with NAT gateway. VPC will contain a group of EC2 instances. All instances configure themselves at startup by downloading bootstrap script from S3 bucket with policy that allows access from customers Amazon EC2 instances and then deploys application through GIT.  SA is tasked with solution that provides highest level of security regarding network connectivity to EC2 instances. How can SA design infrastructure?
        - **Place the Amazon EC2 instances in a private subnet, with no EIPs; route outgoing traffic through the NAT gateway**
    EC2 instances running in private subnets of a VPC can now have controlled access to S3 buckets, objects, and API functions that are in the same region as the VPC.

11. Data processing application runs on i3.large EC2 instance with single 100GB EBS gp2 volume. App stores temporary data in small database (less than 30 GB) located on EBS root volume. App is struggling to process data fast enough, and SA has determined that the I/O speed of temporary database is the bottleneck. What is the most cost-efficient way to improve DB response times?
       - **Move the temporary database onto instance storage.**

12. Web server will be provisioned on two EC2 instances with an ALB. Which config will allow traffic on HTTP and HTTPS when configuring security group to apply to each of these servers?
        - **Allow incoming traffic to HTTP and HTTPS ports.**

13. A company requires OS permission on a relational database server. What should a SA suggest as a configuration for highly scalable database architecture?
       - **Multiple EC2 instances in a database replication configuration that uses two Availability Zones.**

14. SA is concerned that the current security group rules for a DB tier are too permissive and may permit requests that should be restricted. Below are current security group permissions for the DB tier. Protocol: TCP, Port range: 1433 (MS SQL), Source: ALL. Currently, the only identified resource that needs to connect to the DB is the application tier consisting of an auto scaling group of EC2 instances. What changes can be made to this security group that would offer the users LEAST privilege?
       - **Change the source to the security group ID attached to the application instances.**

15. Company has a website running on EC2. The application DNS name points to an Elastic IP address associated with the EC2 instance. In the event of an attack on the website coming from a specific IP address, the company wants a way to block offending IP addresses. Which tool or service should SA recommend to block IP address?
       - **Network ACL**

16. Company wants to migrate a three-tier web app to AWS. Company wants to control the placement of the instances and have visibility into underlying sockets and cores for licensing purposes. Which compute model should a SA choose to accomplish this task?
       - **EC2 Dedicated Hosts**

17. SA is designing a service that must have 4 EC2 instances running between 8am and 6pm daily. Service requires one EC2 instance outside of those hours. What is the most cost effective way to provide enough compute?
        - **Use one Amazon EC2 Reserved Instance and use an Auto Scaling Group scheduled action to add three EC2 On-Demand instances at 7:30 AM and remove three instances at 6:10 PM.**

18. App is running on EC2 instances behind an ALB. The instances run in an Auto Scaling group across multiple AZ. Four instances are required to handle predictable traffic load. SA wants to ensure that the operation is fault tolerant up to the loss of one AZ. Which is the most cost efficient way to meet these requirements?
       - **Deploy two instances in each of three Availability Zones.**

19. SA needs to design a centralized logging solution for a group of web applications running on EC2 instances. The sol requires minimal development effort due to budget constraints. Which of the following should the Architect recommend?
       - **Install and configure Amazon CloudWatch Logs agent in the Amazon EC2 instances.**

20. Employees from several companies use an application once a year during a specific 30-day period. The periods are different for each company. Traffic to the application spikes during these 30-day periods. How can the application be designed to handle these traffic spikes?
       - **Use an Auto Scaling group to scale the number of EC2 instances to match the site traffic.**

21. SA is about to deploy an API on multiple EC2 instances in Auto Scaling group behind an ELB. The support team has following operational requirements: 
They get an alert when the requests per second go over 50,000, they get an alert when latency goes over 5 seconds, they can validate how many times a day users call the API requesting highly sensitive data. Which combination of steps does SA need to take to satisfy these operational requirements?
       - **Create a custom CloudWatch metric to monitor the API for data access.**
       - **Ensure that detailed monitoring for the EC2 instances is enabled.**     

22. An interactive, dynamic web runs on EC2 instances in single subnet behind an ELB CLB. Which design changes will make the site more highly available?
       - **Move some Amazon EC2 instances to a subnet in a different way.**

23. Multi-tiered architecture for an application with public facing web-tier. EC2 instances running in application tier to not be accessible directly from the internet.
       - **Deploy the web and application instances in a private subnet. Provision an Application Load Balancer in the public subnet. Install an internet gateway and use security groups to control communications between the layers.**

24. Company creates business critical 3D images every night. Images are batch-processed every Friday and require an uninterrupted 48 hours to complete. What is the most cost effective EC2 pricing model for this scenario?
       - **Scheduled Reserved Instances**

25. SA is designing a highly available web application on AWS. The data served on the website is dynamic and is pulled from DynamoDB. All users are geographically close to one another. How can SA make the application highly available?
        - **Host the application on EC2 instances across multiple Availability Zones. Use an Auto Scaling group coupled with an Application Load Balancer.**

26. App runs on EC2 instance behind ELB ALB. Instances run in an EC2 Auto scaling group across multiple AZ. App provides a RESTful interface with both syn and async operations. The asyn operations require up to 5 mins to complete. Although app must remain available at all times, after bus hours, the traffic going to app is greatly reduced and often results in Auto Scaling group running the minimum number fo On-Demand Instances. What should SA recommend to optimize the cost of the environment after business hours?
       - **Purchase Reserved Instances for the minimum number of Auto Scaling instances.**
  
  application must remain available at all times" = reserved instances for the time it is running.

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

  Scenarios:

  1. Application runs on VPC  on EC2 instance behind ALB. Traffic to EC2 instances must be limited to traffic from ALB. Based on these requirements, the security group configuration must allow traffic from:
       - **the security group attached to the Application Load Balancer.**

  2. SA is designing architecture for a web application that will be hosted on AWS. Internet users will access applications using HTTP and HTTPS. How to design traffic control requirements?
        - **Allow inbound ports for HTTP and HTTPS in the security group used by the web servers.**
  
  3. SA is designing multi-tier application consisting of ALB, RDS instance, Auto scaling group on EC2 instance. Each tier is in separate subnet. There are some EC2 instances in the subnet that belong to another application. RDS database instance should accept traffic only from EC2 instances in Auto scaling group. What to do to meet these requirements?
        - **Configure the inbound rules on the security group associated with the RDS database instance. Set the source to the security group associated with instances in the Auto Scaling group**
  
  4. SA is designing an application that will run on ECS behind an ALB. For security reasons, EC2 host instances for the ECS cluster are in private subnet. What to do to ensure the incoming traffic to host instances is from ALB only?
        - **Modify the security group used by the EC2 cluster to allow incoming traffic from the security group used by the ALB only.**
  
  5. Organization is deploying ElastiCache for Redis and requires password protection to improve their data security posture. Which solution should a SA recommend?
        - **Redis Auth**
  
  6. Web app is running on EC2 instances behind an Elastic Load balancing ALB. EC2 instances should receive no traffic, except for web requests to the application. Based on these requirements, what security group rules should be put on the EC2 instances?
       - **An inbound rule allowing traffic from the security group attached to the ALB**

  7. SA is designing three-tier web app that includes an Auto Scaling group of EC2 instances running behind ELB Classic Load Balancer. Sec team requires that all web servers to be accessible only through a Load Balancer, and that none of the web servers are directly accessible from Internet. How should Architect meet these requirements?
       - **Configure the web tier security group to allow only traffic from the ELB Classic Load Balancer.**
  
  8. SA needs to allow developers to have SSH connectivity to web servers. Requirements are as follows: limit access to users origination from corporate network, web servers cannot have SSH access directly from the internet, web servers reside in a private subnet. Which combination of steps must SA complete to meet these requirements?
        - **Create a bastion host with security group rules that only allow traffic from the corporate network.**
        - **Configure the web servers' security group to allow SSH traffic from a bastion host.**
  
  9. Two auto scaling apps, A and B, currently run within a shared set of subnets. SA wants to make sure that app A can make request to app B, but app B should be denied from making request to app A. Which is the simplest sol to achieve this policy?
        - **Using security groups that reference the security groups of the other application**
  
  10. Company hosts a two-tier app that consists of publicly accessible web server that communicates with private database. Only HTTPS port 443 traffic to the web server must be allowed from the internet. Which of the following options will achieve these requirements?
        - **Security group rule that allows inbound Internet traffic for port 443.**
        - **Network ACL rule that allows port 443 inbound and all ports outbound for Internet traffic.**
      A = Inbound on Port 443 is fine as Security Groups are stateful (allow the traffic to return to the remote party without an explicit rule)
      C = Inbound 443 on the Network ACL is a given, however, Network ACL's are stateless and therefore need configuring. In this case, the web server replies on a range of ports and therefore it is required that they are all open rather than simply 443 as stated in answer E.

  11. SA is designing a three-tier web app. The architect wants to restrict access to the DB tier to accept traffic from app servers only. However, these app servers are in an Auto Scaling group and may vary in quantity. How should SA configure DB servers to meet the requirements?
        - **Configure the database security group to allow database traffic from the application server security group.**
        simple way to execute the solution via security group.
  
## CloudTrail

1. Once you enable Cloudtrail on a root account, it will log all API interactions on the account and it will also propagate automatically to any new region defined in the account.

Scenarios:

1. Client reports they want to see an audit log of any changes made to AWS resources in their account. What can clients do to achieve this?
       - **Enable AWS CloudTrail logs to be delivered to an Amazon S3 bucket**
       CloudTrail ~ audit

2. Company is using AWS KMS to secure their RDS DB.  An auditor has recommended that the company log all use of their AWS KMS keys. What is the SIMPLEST solution?
       - **Use AWS CloudTrail to log AWS KMS key usage.**
Built-in auditing
AWS KMS is integrated with AWS CloudTrail to record all API requests, including key management actions and usage of your keys.

3. SA needs to design a solution that will enable security team to delete, review and perform root cause analysis of security incidents that occur in cloud environment. 
 - SA must provide a centralized view of all API events for current and future AWS regions.
 - How should SA accomplish this task?
       - **Enable AWS CloudTrail by creating a new trail and apply the trail to all regions.**

## Kinesis

1. Kinesis=event=1000 request
2. Amazon Kinesis Data Firehose is the easiest way to reliably load streaming data into data lakes, data stores, and analytics tools. ... It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration.

Scenarios:

1. SA designed a system based on Kinesis Data Streams.
  - After workflow was put in production, company noticed it performed slowly and identified Kinesis Data Streams as the problem.
  - One of the streams has a total of 10 Mb/s throughput.
  - What should SA do to improve performance?
       - **Run the UpdateShardCount command to increase the number of shards in the stream**
  Shards scale linearly, so adding shards to a stream will add 1MB per second of ingestion, and emit data at a rate of 2MB per second for every shard added. Ten shards will scale a stream to handle 10MB (10,000 PUTs) of ingress, and 20MB of data egress per second"

2. Company is building a critical ingestion service on AWS that will receive 1,000 incoming events per second.
 - The events must be processed in order, and no events may be lost.
 - Multiple applications need to process each event.
 - The company will expose the service as RESTful calls through API gateway.
 - What should SA use to receive the events based on these requirements?
       - **Amazon Kinesis Data Stream**
   Kinesis=event=1000 request
   
3. SA is designing a microservice to process records from Kinesis Streams. 
 - The metadata must be stored in Amazon DynamoDB. 
 - Microservice must be capable of concurrently processing 10,000 records daily as they arrive in the Kinesis stream.
 - The MOST scalable way to design the microservice is:
      - **As a Docker container running on Amazon ECS.**
  Docker makes it easy to build and run distributed microservices architecures, deploy your code with standardized continuous integration and delivery pipelines, build highly-scalable data processing systems, and create fully-managed platforms for your developers. Docker also provides big data processing as a service. (https://aws.amazon.com/docker/)

4. Org must process a stream of large-volume hashtag data in real time and needs to run custom SQL queries on the data to get insights on certain tags. 
 - The org needs this solution to be elastic and does not want to manage clusters. 
 - Which of the AWS services meets these requirements?
       - **Amazon Kinesis Data Analytics**
  Input – The streaming source for your application. You can select either a Kinesis data stream or a Kinesis Data Firehose data delivery stream as the streaming source. In the input configuration, you map the streaming source to an in-application input stream. The in-application stream is like a continuously updating table upon which you can perform the SELECT and INSERT SQL operations. In your application code, you can create additional in-application streams to store intermediate query results.

5. Online company wants to conduct real-time sentiment analysis about its products from its social media channels using SQL. 
 - Which of the following solutions has the LOWEST cost and operational behavior?
        - **Configure the input stream using Amazon Kinesis Data Streams. Use Amazon Kinesis Data Analytics to write SQL queries against the stream.**

6. Company must collect temperature data from thousands of remote weather divisions. 
 - The company must also store this data in a data warehouse to run aggregations and visualizations.
 - Which services will meet these requirements?
       - **Amazon Kinesis Data Firehouse**
       - **Amazon Redshift**
    collect = kinesis
    Data warehouse = redshift

7. App is running on EC2 instance in private subnet.
 - App needs to read and write data into Kinesis Data Streams, and corporate policy requires that this traffic should not go to the Internet.
 - How can these requirements be met?
        - **Configure an interface VPC endpoint for Kinesis and route all traffic to Kinesis through the gateway VPC endpoint.**
  
8. Company website receives 50,000 requests each second, and company wants to use multiple apps to analyze navigation patterns of the users on their website so that the experience can be personalized.
 - What can SA use to collect page clicks for the website and process them sequentially for each user?
        - **Amazon Kinesis Stream**

9. A user is testing a new service that receives location updates from 3,600 rental cars every hour. 
 - Which service will collect data and autoscale to accommodate production overload?
        - **Amazon Kinesis Firehose**
    
10. Manufacturing company captures data from machines running at customer sites. Currently, thousands of machines send data every 5 mins, and this is expected to grow to hundreds of thousands of machines in the near future.
 - The data is logged with the intent to be analyzed in the future as needed. What is the SIMPLEST method to store this streaming data at scale?
       - **Create an Amazon Kinesis Firehouse delivery stream to store the data in Amazon S3.**
  
## API Gateway
1. Lambda and API because it has to scale based on demand. SPOT instances do work for stateless apps, should be in a comparable price versus lambda but the issue is elasticity and how these instances are going to be available (or the lack of certainty they will).
2. Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. With a few clicks in the AWS Management Console, you can create REST and WebSocket APIs that act as a “front door” for applications to access data, business logic, or functionality from your backend services, such as workloads running on Amazon Elastic Compute Cloud (Amazon EC2), code running on AWS Lambda, any web application, or real-time communication applications.
API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management. API Gateway has no minimum fees or startup costs.

Scenario:

1. Company developing new stateless web service with low memory requirements. Service needs to scale based on demand.
 - What is most cost effective solution?
       - **Deploy the application onto AWS Lambda with access through Amazon API Gateway**

2. App running on AWS lambda requires an API key to access third party service. 
 - The key must be stored securely with audited access to Lambda function only.
 - What is the most secure way to store the key?
       - **As a secure string in AWS Systems Manager Parameter Store**

3. SA must select most cost efficient architecture for a service that responds to web requests.
 - These web requests are small and query DynamoDB table.
 - The request rate ranges from zero to several hundred each second, without any predictable patterns.
 - What is most cost efficient architecture for this service?
       - **API Gateway/AWS Lambda**

3. Company plans to migrate a website to AWS to use serverless architecture. Website contains both static and dynamic content and is accessed by users across the world.
 - The website should maintain sessions for returning users to improve the user experience.
 - Which service should SA use for cost efficient solution with Lowest latency?
       - **Amazon S3, Amazon CloudFront, AWS Lambda, Amazon API Gateway, and Amazon DynamoDB.**
CloudFront is needed because they want to cache static & dynamic content... this is why.

4. Part of migration strategy, SA needs to analyze workloads that can be optimized for performance and cost.
 - SA has identified a stateless app that serves static content as potential candidate to move to cloud. 
 - SA has flexibility to choose an identity sol between facebook, twitter and amazon.
 - Which AWS sol offers flexibility and ease of use, and the LEAST operational overhead for this migration?
       - **Use Amazon Cognito for managing identities, and migrate the application to run on Amazon S3, Amazon API Gateway, and AWS Lambda.**
Amazon Cognito identity pools integrate with Facebook to provide federated authentication for your mobile application users.
EC2 vs Lambda = Least operation overhead - go for server less i.e. Lambda

5. Company has popular multi-player mobile game hosted in its on-premises datacenter. 
 - The current infrastructure can no longer keep up with the demand and the company is considering a move to the cloud.
 - Which solution should a SA recommend as the MOST scalable and cost-effective solution to meet these needs?
       - **AWS Lambda and Amazon API Gateway**
  API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management. API Gateway has no minimum fees or startup costs. You pay only for the API calls you receive and the amount of data transferred out and, with the API Gateway tiered pricing model, you can reduce your cost as your API usage scales.

  Aws Lambda:
  Case study highlights"from several hours to just over 10 seconds, and reduced infrastructure and operational costs." Most gaming website using Lambda.
  https://aws.amazon.com/lambda/resources/customer-case-studies/

6. A retail company has sensors placed in its physical retail stores.
 - The sensors send messages over HTTP when customers interact with in-store product displays. 
 - SA needs to implement a system for processing those sensor messages, the results must be available for the Data analysis team.
 - Which architecture to use to meet these requirements?
       - **Implement an Amazon API Gateway to server as the HTTP endpoint. Have the API Gateway trigger an AWS Lambda function to process the messages, and save the results to an Amazon DynamoDB table.**

7. SA is developing a new web application on AWS. 
 - The architect expects the app to become very popular, so the app must scale to support the load. 
 - Architect wants to focus on software development and deploying new features without provisioning or managing instances.
 - What solution is appropriate?
       - **Amazon API Gateway and AWS Lambda**
  Both Lambda and API Gateway are serverless services, AWS will manage Provisioning and managing.

8. As part of securing API layer built on API gateway, SA has to authorize users who are currently authenticated by an existing identity provider.
 - The users must be denied access for a period of one hour after three unsuccessful attempts. How can SA meet these requirements?
        - **Use an API Gateway custom authorizer to invoke an AWS Lambda function to validate each user's identity.**

9. Customers own a simple API for their website that receives about 1,000 requests each day and has an average response time of 50 ms. It is currently hosted on one c4.large instance. Which changes to the architecture will provide high availability at the LOWEST cost?
        - **Recreate the API using Amazon API Gateway and use AWS Lambda as the service backend.**
  
## NAT Gateway

1. Note If you have resources in multiple Availability Zones and they share one NAT gateway, in the event that the NAT gateway’s Availability Zone is down, resources in the other Availability Zones lose internet access. To create an Availability Zone-independent architecture, create a NAT gateway in each Availability Zone and configure your routing to ensure that resources use the NAT gateway in the same Availability Zone. 
2. Connect to the Internet using Network Address Translation (private subnets) – Private subnets can be used for instances that you do not want to be directly addressable from the Internet. Instances in a private subnet can access the Internet without exposing their private IP address by routing their traffic through a Network Address Translation (NAT) gateway in a public subnet.
3. Reason, The web Servers in web subnet can get Internet via ELB or IGW but private cannot go through ELB and IGW. They either need Nat Instance or Nat Gateway. since NAT instances are not mentioned then it must be Nat Gateway.
4. you must always create your NAT gateway in a public subnet that already has a defined route to 0.0.0.0/0 through an internet gateway. and assign an Elastic IP to the NAT gateway.
5. You have a limit on the number of NAT gateways you can create in an Availability Zone. 

Scenarios:

1. Application runs on EC2 instance in multiple AZ behind an ALB.
 - Load balancer is in public subnets, EC2 instances are in private subnets and must not be accessible from the internet.
 - EC2 instance must call external services on the internet.
 - If one AZ become unavailable, remaining EC2 instances must still be able to call external services.
        - **Create a NAT gateway in each AZ. Update the route tables for each private subnet to direct internet-bound traffic to the NAT gateway.**

2. Design high performance computing job running on EC2 instances in private subnets.
 - To allow application to download patches, infrastructure must be altered to allow instances to access external endpoints.
 - Changes to infrastructure must involve minimal ongoing systems management effort. 
 - What allows EC2 instances to access endpoint while meeting these requirements?
       - **NAT gateway**

3. Application has components running in public and private subnet. Components within the private subnet must connect to the internet to receive updates. How to do this without moving components into public subnet.
       - **Add a NAT gateway to the public subnet and update the private subnet route table.**

4. Convert single point of failure to highly available configuration. Current architecture contains Amazon EC2 instances with databases running in one AZ. Web tier resources have not been given public address, but still need internet access. Which solution to use to maintain high availability?
       - **Use ELB Classic Load Balancer with the web tier. Deploy EC2 instances in two Availability Zones and enable Multi-AZ RDS. Deploy NAT gateways in both Availability Zones.**

5. SA is designing application in AWS. Architect must not expose application or database tier over Internet for security reasons. App must be low-cost and have scalable front end. Database and application tier must have only one way internet access to download software and patch updates. Which solution helps to meet these requirements?
       - **Use an ELB Classic Load Balancer as the front end for the application tier, and a NAT Gateway to allow Internet access for private resources.**

6. App has web tier that runs on EC2 instances in public subnet. App tier instances run in private subnets across two AZ. All traffic is IPv4 only, each subnet has its own custom route table. New feature requires that app tier instances can call external service over the internet, however they must still not be accessible to internet traffic. What should be done to allow app servers to connect to internet maintain high availability and minimize admin overhead?
       - **Add an Amazon NAT Gateway to each public subnet. Alter each private subnet's route table to include a route from 0.0.0.0/0 to the NAT Gateway in the same Availability Zone.**

7. SA is working on PCI compliant architecture that needs to call an external service providers API. The external provider requires IP whitelisting to verify calling party. How should SA provide external party with IP addresses for whitelisting?
        - **Deploy the Lambda function in private subnets and route outbound traffic through a NAT gateway. Provide the NAT gateway's Elastic IP address to the external service provider.**

8. A company has instances in private subnets that require outbound access to the Internet. This requires:

         - **Updating the route table associated with the subnet to point internet traffic through a NAT gateway.**

9. SA is building multi-tier website. The web servers will be in public subnet, and the DB server will be in private subnet. Only web servers can be accessed from internet. DB servers must have internet access for software updates. Which solution meets the requirements?
        - **Use a NAT Gateway.**

10. SA is designing a web app. The web and app tiers need to access the Internet, but they cannot be accessed from the Internet. Which of the following steps is required?
        - **Launch a NAT gateway in the public subnet and add a route to it from the private subnet.**

11. SA plans to migrate NAT instances to NAT gateway. SA has NAT instances with scripts to manage high availability. What is the MOST efficient method to achieve similar high availability with NAT gateway?
        - **Launch a NAT gateway in each Availability Zone.**

You have a limit on the number of NAT gateways you can create in an Availability Zone. For more information, see Amazon VPC Limits.

12. Internet facing multi-tier web app must be highly available. ELB CLB is deployed in front of the web tier. EC2 instances at the web app tier are deployed evenly across two AZ. DB is deployed using multi AZ. A NAT instance is launched for EC2 instances and DB resources to access the internet. These instances are not assigned with public IP addresses. Which component poses a potential single point of failure in this architecture? 
        - **NAT instance**

## Load Balancers

1. An application load balancer can take the place of multiple individual classic load balancers to load balance microservices (e.g. running on AWS ECS) saving costs.
Most importantly though, COST. Application Load Balancers cost $0.0252 per hour. Classic Load Balancers cost $0.028 per hour.                           A great article is here to refer: https://medium.com/cognitoiq/how-cognitoiq-are-using-application-load-balancers-to-cut-elastic-load-balancing-cost-by-90-78d4e980624b

Scenarios:

1. Organization hosts 10 microservices, each in autoscaling group behind individual CLB. Each EC2 instance running at optimal load. Which will allow organization to reduce costs without impacting performance?
        - **Replace the Classic Load Balancers with a single Application Load Balancer.**

2. SA plans to migrate load balancer tier from data center to AWS. Several websites have multiple domains that require secure load balancing. SA decides to use Elastic Load balancing application load balancers. What is most efficient method to achieve secure communication?
       - **Create an SNI certificate and upload it to the Application Load Balancer**

3. Company needs to use AWS resources to expand capacity for a website hosted in on-premises data center. AWS resources will include load balancers, auto scaling and EC2 instances that will access on premises database. Network connectivity has been established but no traffic is going to AWS environment. How should Route 53 be configured to distribute load to AWS environment?
       - **Set up a weighted routing policy, distributing the workload between the load balancer and the on-premises environment.**
       - **Set up an A record to point the DNS name to the IP address of the load balancer.**

4. SA needs to deploy HTTP/HTTPS service on EC2 instances with support for WebSockets using load balancers. How can architect meet these requirements?
      - **Configure an Application Load Balancer.**
Http/Https are application layer (osi layer 7) 

## ElasticCache

1. Session data - Elasticache.
2. Enabling ElastiCache Multi-AZ with automatic failover on your Redis cluster (in the API and CLI, replication group) improves your fault tolerance. This is true particularly in cases where your cluster's read/write primary cluster becomes unreachable or fails for any reason. Multi-AZ with automatic failover is only supported on Redis clusters that support replication
3. Scheduled Instances are a good choice for workloads that do not run continuously, but do run on a regular schedule. For example, you can use Scheduled Instances for an application that runs during business hours or for batch processing that runs at the end of the week.
If you require a capacity reservation on a continuous basis, Reserved Instances might meet your needs and decrease costs.


Scenarios:

1. Website keeps a record of user actions using GUID(globally unique identifier) retrieved from amazon aurora in place of username within the audit record. GUID content must not leave the Amazon VPC.
 - With increase in web traffic, web servers and Aurora read replica has also increased to keep up with the user record reads for the GUID.
 - To reduce read replica number while improving performance.
        - **Deploy a Amazon ElastiCache for Redis server into the infrastructure and store the user name and GUID there. Retrieve GUID from ElastiCache  when required.**
        (ElasticCache to improve performance and reduce read replica to retrieve GUID)

2. Design application that has millions of users. SA needs to store session data. Which option is most performant?
       - **Amazon ElastiCache**
       
3. Gaming application is heavily dependent on caching and uses ElastiCache for Redis. The application performance was recently degraded due to failure of cache node. What should SA recommend to minimize performance degradation in the future?
       - **Configure ElastiCache Multi-AZ with automatic failover**

4. Company wants to improve the performance of their web application after receiving customer complaints. An analysis concluded that the same complex database queries were causing increased latency. What should SA recommend to improve the applications performance?
       - **Integrate Amazon ElastiCache into the application.**

5. Social networking portal experiences latency and throughput issues due to an increased number of users. App servers use very large datasets from RDS DB, which creates a performance bottleneck on the DB. Which AWS service should be used to improve performance?
       - **Amazon ElastiCache**

6. Online retailer has series of flash sales occurring every friday. Sales traffic will increase during slaes only and platform handles increased load. Platform is three tier application. Web tier runs on EC2 instances behind ALB. Amazon Cloudfront is used to reduce web server load, but many requests for dynamic content must to go the web servers. What to do to web tier to reduce costs without impacting performance or reliability?
       - **Purchase scheduled Reserved Instances.**
"occurring every Friday" makes it a good candidate for scheduled reserved instances.

7. SA is building a wordpress based web application hosted on AWS using EC2. This application serves as a blog for international internet security company. App must be geographically redundant and scalable. It must separate EC2 web servers from private RDS DB, highly available and it must support dynamic port routing. Which combination of AWS services or capabilities meet these requirements?
        - **Amazon Route 53, Auto Scaling with an Application Load Balancer, and Amazon CloudFront**

8. SA is building a wordpress based web application hosted on AWS using EC2. This application serves as a blog for an international internet security company. App must be geographically redundant and scalable. It must separate EC2 web servers from private RDS DB, highly available and it must support dynamic port routing. Which combination of AWS services or capabilities meet these requirements?
       - **Amazon Route 53, Auto Scaling with an Application Load Balancer, and Amazon CloudFront**

## SQS

1. FIFO will deliver atleast once and no duplicates.
2. Default queue could deliver twice can have duplicates.
3. Decoupling this element and having it processed by separate workers allows the rest of the system to not suffer the impact of the slow-running worker.
https://medium.com/@pablo.iorio/synchronous-and-asynchronous-aws-decoupling-solutions-1bbe74697db9


Scenarios:

1. User submit requests to a service that takes several minutes to process. SA needs to ensure that these requests are processed at least once, and that the service has the ability to handle large increases in number of requests. How should these requirements be met?
       - **Put the requests into an Amazon SQS queue and configure Amazon EC2 instances to poll the queue**

2. App uses an SQS queue as transport mechanism to deliver data to a group of EC2 instances for processing. Application owner wants to add mechanism to archive incoming data without modifying application code on EC2 instances. How can this application be re-architected to archive the data without modifying the processing instances?
        - **Use an Amazon SNS topic to fan out the data to the SQS queue in addition to a Lambda function that records the data to an S3 bucket.**

3. An organization runs an online voting system for TV program. During broadcasts, hundreds of thousands of votes are submitted within minutes and sent too a front end fleet of auto-scaled EC2 instances. The EC2 instances push the votes to an RDBMS database. The DB is unable to keep up with front-end connection requests. What is the MOST efficient and cost effective way of ensuring that votes are processed in timely manner?
         - **Each front-end node should send votes to an Amazon SQS queue. Provision worker instances to read the SQS queue and process the message information into RDBMS database.**

4. Restaurant reservation application needs to maintain waiting list. When a customer tries to reserve a table, and none are available, customer must be put on waiting list, and the application must notify the customer when a table becomes free. What service should the SA recommend to ensure that the system respects the order in which the customer requests are put onto the waiting list?
        - **A FIFO queue in Amazon SQS**

5. SA is designing customer order processing applications that will likely have high usage spikes. What should the Architect do to ensure that customer orders are not lost before being written to an RDS database?
        - **Have the orders written into an Amazon SQS queue.**
        - **Scale the number of processing nodes based on pending order volume.**

6. Legacy app currently send messages through single EC2 instance, which then routes the messages to the appropriate destinations. The EC2 instance is a bottleneck and single point of failure, so company would like to address these issues. Which services could address this architectural use cases?
        - **Amazon SNS**
        - **Amazon SQS**

7. App relies on messages being sent and received in order. The volume will never exceed more than 300 transactions each second. Which service should be used?
        - **Amazon SQS**

8. Website experiences unpredictable traffic. During peak traffic times, the DB is unable to keep up with the write request.Which AWS service will help decouple the web app from DB?
        - **Amazon SQS**

9. Company has web application to make requests to backend API service. API service is behind ELB running on EC2 instances. Most API service endpoint calls finish quickly but one endpoint that makes calls to create objects  in external service takes a long time to complete. These long running calls are causing client timeouts and increasing overall system latency. What to do to minimize system throughput impact of slow-running endpoint?
         - **Use Amazon SQS to offload the long-running requests for asynchronous processing by separate workers.**

10. SA designs three-tier web application to allow customers to upload pictures from mobile application. App will generate thumbnail of picture and return message to user confirming image successfully uploaded. Generation of thumbnail may take up to 5 seconds. To provide a sub second response time to the customers uploading the images, SA wants to separate web tier from application tier. Which service allow the presentation tier to dispatch request to application tier?
          - **Amazon SQS**

11. When designing SQS message processing solution, messages in queue must be processed before maximum retention time has elapsed. Which actions will meet this requirement?
          - **Use Amazon EC2 instances in an Auto Scaling group with scaling triggered based on the queue length**
          - **Increase the SQS queue attribute for the message retention period**

12. An application publishes SNS messages in response to several events. An Lambda function subscribes to these messages. Occasionally the function will fail while processing a message, so the original event message must be preserved for root cause analysis. What architecture will meet these requirements without changing the workflow?
          - **Configure a Dead Letter Queue for the Amazon SNS topic.**
       
## Route 53

1. MultiValue Answer Routing would fit to random queries with health checks.
2. You can use the cloudfront with Route 53 Geolocation Routing. But the location wise content delivery is already enabled in cloudfront, so geolocation policy wont help that much. If you are not using cloudfront and you want to distribute traffic based on user location, then you can use Route53
3. For an active-active system, Geolocation, Geoproximity, Latency, Multivalue, or Weighted policies would work.
https://medium.com/dazn-tech/how-to-implement-the-perfect-failover-strategy-using-amazon-route53-1cc4b19fa9c7


Scenarios:

1. SA has five web servers serving requests for a domain. Which of the following Route 53 routing policies can distribute traffic randomly among all healthy web servers?
         - **Multivalue Answer**

2. SA is designing a solution for dynamic website, “example.com” that is deployed in two regions: Tokyo, Japan and Sydney, Australia. Architect wants to ensure that user located in Australia are directed to website deployed in sydney and users in Japan are redirected to website in tokyo when they browse to example.com. Which service should the architect use to achieve this goal with LEAST administrative effort?
         - **Amazon Route 53**

3. Company is designing a failover strategy in Route 53 for its resources between two AWS regions. Company must have ability to route users traffic to the region with least latency, and if both regions are healthy, Route 53 should route traffic to resources in both regions. Which strategy should SA recommend?
        - **Configure active-active failover using Route 53 latency DNS records.**

4. Company is launching a static website using zone apex(mycompany.com). The company wants to use Route 53 for DNS. Which steps should the company perform to implement scalable and cost-effective solution?
        - **Serve the website from an Amazon S3 bucket, and map a Route 53 alias record to the website endpoint.**
        - **Create a Route 53 hosted zone, and set the NS records of the domain to use Route 53 name servers.**

5. SA is designing a web app that will be hosted on EC2 instances in public subnet. The web app uses mySQL DB in private subnet. DB should be accessible to DBA. Which of the following SA should recommend?
        - **Create a bastion host in a public subnet, and use the bastion host to connect to the database.**
        - **Create an IPSec VPN tunnel between the customer site and the VPC, and use the VPN tunnel to connect to the database.**

## MQ

1. Amazon MQ is a managed message broker service for Apache ActiveMQ that makes it easy to set up and operate message brokers in the cloud. Message brokers allow different software systems–often using different programming languages, and on different platforms–to communicate and exchange information. With Amazon MQ, you can use industry standard APIs and protocols for messaging, including JMS, NMS, AMQP, STOMP, MQTT, and WebSocket. You can easily move from any message broker that uses these standards to Amazon MQ because you don’t have to rewrite any messaging code in your applications.

Scenarios:

1. Company migration an on-premises application to AWS. App uses corporate message broker, passing messages between layers by using MQTT protocol. Due to time and budget constraint, company cannot rewrite the application and cannot manage new message broker on EC2 instances. Which service to use to allow customer to migrate app to AWS?
        - **Amazon MQ**

## Network Load Balancer

1. Choose an Application Load Balancer when you need a flexible feature set for your web applications with HTTP and HTTPS traffic.
Choose a Network Load Balancer when you need ultra-high performance, TLS offloading at scale, centralized certificate deployment, support for UDP, and static IP addresses for your application.
2. Host-based routing use host conditions to define rules that forward requests to different target groups based on the host name in the host header. This enables ALB to support multiple domains using a single load balancer.
Path-based routing use path conditions to define rules that forward requests to different target groups based on the URL in the request. Each path condition has one path pattern. If the URL in a request matches the path pattern in a listener rule exactly, the request is routed using that rule.
3. The Health check is configured for ELB and failing, however the Auto Scaling needs to use the ELB health check, in addition to the system checks, to determine if the instance in healthy. Hence the Auto Scaling group needs to be updated to use ELB health check for terminating the instances.
4. Application Load Balancer. Keywords: Distribute based on URL = Path based routing for ALB
5. For example, previously Amazon S3 performance guidelines recommended randomizing prefix naming with hashed characters to optimize performance for frequent data retrieval. You no longer have to randomize prefix naming for performance, and can use sequential date-based naming for your prefixes.

Scenarios:

1. Customer has application used by enterprise customers outside of AWS. Some of them use legacy firewalls that cannot whitelist by DNS name, but whitelist based only on IP address. App is deployed in two AZ, with one EC2 instance in each that has Elastic IP address. Customer wants to whitelist only two IP addresses, but the two existing EC2 instances cannot sustain the amount of traffic. What can SA do to support customer and allow for more capacity?
       - **Create a Network Load Balancer with an interface in each subnet, and assign a static IP address to each subnet.**
       - **Switch the two existing EC2 instances for an Auto Scaling group, and register them with the Network Load Balancer.**

2. SA is designing web application that runs on EC2 instances behind a load balancer. All data in transit must be encrypted. Which solutions will meet the encryption requirement?
        - **Use a Network Load Balancer (NLB) with a TCP listener, then terminate SSL on EC2 instances.**
        - **Use an Application Load Balancer (ALB) with an HTTPS listener, then install SSL certificates on the ALB and EC2 instances.**

3. Large media site have multiple applications in ECS. SA needs to use content metadata and route traffic to specific services. What is the most efficient method to perform this task?
        - **Use an AWS Application Load Balancer with host-based routing option to route traffic to the correct service.**

4. A client has setup an Auto Scaling group associated with load balancer. Client noticed that instances launched by Auto Scaling group are reported unhealthy as a result of ELB health check, but these unhealthy instances are not being terminated. What can solutions architect do to ensure that the instances marked unhealthy will be terminated and replaced?
        - **Change the health check type to ELB for the Auto Scaling group.**

5. Company has asked SA to modify its AWS hosted internal application to allow for load balancing. The customer requests always come from the company domain (example.net). The company requires that incoming HTTP and HTTPS traffic is routed based on the path element of the URL in the request. Which implementation can satisfy all requirements?
        - **Configure an Application Load Balancer with listeners for appropriate path patterns for the target group.**

6. Company needs to capture all client connection information from its ALB every 5 mins. This data will be used to analyze traffic patterns and troubleshoot the application. How can SA meet this requirement?
        - **Enable Access Logs on the Application Load Balancer.**

7. App tier currently hosts two web services on same set of instances, listening on different ports. Which AWS service should SA use to route traffic to the service based on the incoming request path?
        - **AWS Application Load Balancer**

8. SA is designing microservices-based app using ECS. App includes a WebSocket component, and the traffic needs to be distributed between microservices based on the URL. Which service should the Architect choose to distribute the workload?
        - **ELB Application Load Balancer**

9. Admin is hosting an app on single EC2 instance, which users can access by public hostname. Admin is adding a second instance, but does not want users to have to decide between many public hostnames. Which AWS service will decouple users from EC2 instances?
        - **Amazon ELB**

10. SA is designing a highly-available website that is served by multiple web servers hosted outside of AWS. If an instance becomes unresponsive, SA needs to remove it from rotation. What is the MOST efficient way to fulfill this requirement?
       - **Use an Amazon Elastic Load Balancer.**

11. SA is designing sol with Lambda where diff environment require different database passwords. What should SA do to accomplish this in a secure and scalable way?
        - **Use encrypted AWS Lambda environmental variables.**

12. SA is designing the solution to store large quantities of event data in S3. Architect anticipates that the workload will exceed 100 requests each second. What should SA do in S3 to optimizate performance?
        - **Randomize a key name prefix.**


## Miscellaneous

Scenarios:
1. Company is moving to AWS. Management has identified a set of approved AWS services that meet all deployment requirements. The company would like to restrict access to all other unapproved services to which employees would have access. Which sol meets these requirements with LEAST amount of operational overhead?
       - **Configure AWS Organizations. Create an organizational unit (OU) and place all AWS accounts into the OU. Apply a service control policy (SCP) to the OU that denies the use of certain services.**

2. Company has asked SA to ensure that data is protected during data transfer to and from S3. Use of which service will protect the data in transit?
        - **HTTPS**

Use encryption to protect your data
If your use case requires encryption during transmission, Amazon S3 supports the HTTPS protocol, which encrypts data in transit to and from Amazon S3. All AWS SDKs and AWS tools use HTTPS by default.
Note: If you use third-party tools to interact with Amazon S3, contact the developers to confirm if their tools also support the HTTPS protocol.

3. SA is designing a public facing web app for employees to upload images to their social media account. The application consists of multiple EC2 instances behind an ELB, an S3 bucket where uploaded images are stored, and DynamoDB for storing image metadata. Which AWS service can the Architect use to automate the process of updating metadata in the DynamoDB table upon image upload?
          - **AWS Lambda**
lambda can be used when you uploaded an image to S3 and it can trigger lambda function to store image metadata to DynamoDB

4. Which tool analyzes account resources and provides detailed inventory changes over time?
           - **AWS Config**
       AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.

5. After reviewing the logs, startup company noticed large, random spikes in traffic to their web application. The company wants to configure a cost efficient auto scaling solution to support high availability of the web application. Which scaling plan should SA recommend to meet the company’s needs?
      - **Dynamic**
random=>Dynamic

6. A media company has deployed multi-tier architecture on AWS. Web servers are deployed in two AZ using an Auto scaling group with default autoscaling termination policy. The web servers Auto scaling group currently has 15 instances running. Which instances will be terminated first during a scale-in operation?
       - **The instance in the Availability Zone that has most instances.**

There's also a key piece of information here - 15 instances. As you can't have half an instance, we can assume there is an uneven split between the two AZs (probably 7 at one and 8 at the other).
Therefore scenario #1 of https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html will apply because it there will definitely be an AZ to pick from that has mores instances available to terminate first before going on to other criteria for further scale-in actions.

7. Org designs a mobile application for their customers to upload photos to a site. App needs secure login with MFA. Org wants to limite the initial build time and maintenance of the solution. Which solution should SA recommend to meet these requirements?
       - **Use Amazon Cognito Identity with SMS-based MFA.**

Mobile/web app authentication, think cognito. If supports MFA as well

8. A company is developing several critical long running applications hosted on Docker. How should SA design a solution to meet the scalability and orchestration requirements on AWS?
       - **Use Amazon ECS and Service Auto Scaling.**
ECS = Docker

9. A workload consists of downloading an image from S3 bucket, processing the image and moving it to other S3 bucket. EC2 instance runs a scheduled task every hour to perform the operation. How should SA redesign the process so that it is highly available?
       - **Trigger a Lambda function when a new object is uploaded**

10. SA needs to design an architecture for a new, mission critical batch processing billing application. The app is required to run Monday, Wednesday and Friday from 5am to 11am. Which is the MOST cost effective EC2 pricing model?
       - **Scheduled Reserved Instances**

11. SA has a two-tier app with single EC2 instance web server and RDS MySQL Multi-AZ DB instances. The Architect is re-architecting the app for high availability by adding instances in a second AZ. Which additional services will improve the availability of the app?
        - **Auto Scaling group**
        - **ELB Classic Load Balancer**

12. SA is designing a stateful web app that will run for one year (24/7) and then be decommisioned. Load on this platform will be constant, using a number of r4.8x large instances. Key drivers for this system include HA, but elasticity is not required. What is the most cost effective way to purchase compute for this platform?
       - **Standard Reserved Instances**
Keywords: stateful web application = needs constant storage to hold different types of data including session data. Highly Available and cheapest = Standard reserved instances

13. Company is launching marketing campaign on their web tomorrow and expects a significant increase in traffic. Web is designed as multi-tiered web architecture and the increase in traffic could overwhelm current design. What should SA do to minimize effects from potential failure in one or more of tiers?
       - **Use Auto Scaling to keep up with the demand**

14. SA has multi layer app running in VPC. The app has an ELB CLB as front end in public subnet, and an EC2 based reverse proxy that performs content based routing to two backend EC2 instances hosted in private subnet. Architect sees tremendous traffic growth and is concerned that reverse proxy and current backend setup will be insufficient. Which actions should SA take to achieve cost effective sol that ensures the app automatically scales to meet traffic demand?
         - **Add Auto Scaling to the Amazon EC2 backend fleet.**
         - **Replace both the frontend and reverse proxy layers with an ELB Application Load Balancer.**
Due to the reverse proxy being a bottleneck to scalability, we need to replace it with a solution that can perform content-based routing. This means we must use an ALB not a CLB as ALBs support path-based and host-based routing Auto Scaling should be added to the architecture so that the back end EC2 instances do not become a bottleneck. With Auto Scaling instances can be added and removed from the back end fleet as demand changes A Classic Load Balancer cannot perform content-based routing so cannot be used It is unknown how the reverse proxy can be scaled with Auto Scaling however using an ALB with content-based routing is a much better design as it scales automatically and is HA by default Burstable performance instances, which are T3 and T2 instances, are designed to provide a baseline level of CPU performance with the ability to burst to a higher level when required by your workload. CPU performance is not the constraint here and this would not be a cost-effective solution

15. SA is designing a photo app on AWS. Every time a user uploads a photo on S3, the architect must insert a new item to the DynamoDB table. Which AWS managed service is the BEST fit to insert the item?
         - **Lambda@Edge**

16. Company plans to use AWS for all new batch processing workloads. The companys devs use Docker containers for the new batch processing. The system design must accomodate critical and non-critical batch processing workloads 24/7. How should SA design this architecture in a cost-efficient manner?
        - **Use Amazon ECS orchestration and Auto Scaling groups: one with Reserve Instances, one with Spot Instances.**

17. Ecommerce apps are hosted on AWS. The last time a new product was launched, the app experienced a performance issue due to an enormous spike in traffic. Management decided that capacity must be doubled the week after the product is launched. Which is the MOST efficient way for management to ensure that capacity requirements are met?
         - **Add a Step Scaling policy.**

18. A call center app consists of a three-tier app using Auto scaling groups to automatically scale resources as needed. Users report that every morning at 9:00 am the system becomes very slow for about 15 mins. A SA determines that a large percentage of the call center staff starts work at 9:00 AM, so auto scaling does not have enough time to scale out to meet demand. How can the Architect fix the problem?
        - **Create an Autoscaling scheduled action to scale out the necessary resources at 8:30 AM every morning.**










     












  






  





























































