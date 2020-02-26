
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









  






  





























































