
# Table of Contents

[Spot Instances](#Spot-Instance)

[RedShift](#RedShift)

[Storage Gateway](#Storage-Gateway)

[Identity and Access Management (IAM)](#Identity-and-Access-Management-(IAM))

[Amazon Aurora](#Amazon-Aurora)

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
- Use AWS KMS Default customer master key.
13. Redshift spectrum:
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
 - AWS Storage gateway in cached mode for the legacy application storage to write data to S3.

## Identity and Access Management (IAM)

1. Assigning an role is giving an access.
2. IAM users cannnot be attached to EC2 instance but IAM role can be attached to EC2 instance.
3. For AWS service to service interaction without need to create credentials, we use IAM roles. In that case we define a policy list RDS instances, use it to create a role and attach the role to the lambda function.
4. For applications running outside of AWS such as on-premises and third-party automation, it will need access keys, NOT IAM Role

Scenarios:

1. Create a solution where user access to multiple MySQL database is securely managed with short lived connection details.
  - To meet requirements:
  - Create a user account to use AWS-Provided AWSAuthenticationPlugin with IAM.

2. Company hosts a website using API gateway on the front end.
  - There is heavy traffic on website.
  - Company wnats to control access by allowing authenticated traffic only.
  - How should company limit access to authenticated users only?
   - Allow users that are authenticated using Amazon Cognito.
   - Assign permissions in AWS IAM to allow users.

3. SA is designing a new workload where AWS Lambda function will access an DynamoDB table.
  - What is most secure means to granting Lambda function access to DynamoDB table?
   - Create an identity and access management (IAM) role with necessary permissions to access DynamoDB table, and assign the role to Lambda function.

4. Company is creating a web application that will run on an EC2 instance.
  - Application the instance needs access to DynamoDB table for storage.
  - To meet requirement:
    - Create IAM role and assign role to the EC2 instance with permissions to the DynamoDB table.

5. SA is developing a software on AWS that requires access to multiple AWS services, including EC2 instance/
  - This is a security sensitive app, and AWS credentials such as Access Key ID and Secret Access Key need to be protected and cannot be exposed anywhere in the system.
  - To meet this security requirement:
    - Assign IAM role to the EC2 instance.

6. SA is designing  a web app that is running on EC2 instance.
  - App stores data in DynamoDB.
  - SA needs to secure access to DynamoDB table.
  - To achieve secure authorization:
    - Create an IAM role with permissions to write to DynamoDB table.
    - Attach an IAM role to the EC2 instance.

7. SA is designing a lambda function that calls an API to list all running RDS instances. How is request authorized?
  - Create IAM role to the Lambda function with permissions to list all Amazon RDS instances.

8. App runs on EC2 instance must make secure calls to S3 bucket.
  - How to ensure calls are made without exposing credentials?
    - Create an IAM role granting least privilege and assign it to EC2 instance profile.

9. Legacy app running in premises require SA to be able to open a firewall to allow access to several S3 buckets.
  - Architect has a VPN connection to AWS in place.
  - To meet this requirement:
   - Create an IAM role that allows access from corporate network to S3.


- Policy

  - Effect: Allow or Deny

  - Action: Include a list of actions that the policy allows or denies (effect)

  - Resource: a list of resources to which the actions apply

  - Principle:

    - The `Principal` element specifies the user, account, service, or other entity that is allowed or denied access to a resource

    - Mostly for resource-based policy, which attached to services such as SQS, S3, KMS. These services have their own policy management system with same language as IAM


## Amazon Aurora

Scenarios:

1. Bank is writing new softare dependant on DB transcations for write consistency.
  - App will do joins across multiple tables.
  - DB must automatically scale as the amount of data grows.
  - AWS service to run the database.
   - Amazon Aurora.

2. Design Disaster recovery (DR) in separate AWS region from applications primary workload.
  - Multi tier architecture and only RDS instance have frequent changes.
  - App installation takes 60 mins on average.
  - DR have an RPO of less than 90 mins and RTO of less than 30 mins. 
  - To meet these:
   - An Aurora instance as primary database with read replica in DR region.
   - A cross region EC2 AMI copy.

3. App stores data in RDS MySQL DB instance.
  - DB queries consists of read queries, which are overwhelming current database.
  - SA wants to scale the database.
  - What combination of steps to achieve goal?
   - Migrate MySQL database to Aurora.
   - Create read replicas in different AZ.

4. Team launching marketing campaign and the peak database read activity in Aurora for MySQL is expected to increase.
  - SA decides to add two Read replicas to the cluster.
  - How should SA ensure that connections for read activities are load balanced?
   - Reader endpoint for amazon Aurora.

5. SA is creating new relational DB.
  - Compliance team will use DB, and mandates that data content must be stored across three different AZ.
  - Which option to use?
   - Aurora.

6. Company wants to migrate a highly transactional DB to AWS.
  - Requirements state that the DB has more than 6TB of data and will grow exponentially.
  - Which solution should SA recommend?
   - Amazon Aurora.

7. Web app stores all data in RDS Aurora DB instance.
  - SA wants to provide access to the data for detailed report for marketing team, but is concerned that additional load on the DB will affect the performance of web app.
  - How can report be created without affecting performance of application?
    - Create a read replica of the database.

8. Popular e-commerse app runs on AWS.
  - The app encounters performance issues.
  - DB is unable to handle amount of queries and load during peak hours.
  - The DB is running on RDS Aurora engine on the largest instance size available.
  - What should admin do to improve performance?
   - Create one or more read replicas.



