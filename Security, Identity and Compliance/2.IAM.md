## Identity and Access Management (IAM)

1. Assigning an role is giving an access.
2. IAM users cannnot be attached to EC2 instance but IAM role can be attached to EC2 instance.
3. For AWS service to service interaction without need to create credentials, we use IAM roles. In that case we define a policy list RDS instances, use it to create a role and attach the role to the lambda function.
4. For applications running outside of AWS such as on-premises and third-party automation, it will need access keys, NOT IAM Role

Scenes:

1. Create a solution where user access to multiple MySQL database is securely managed with short lived connection details.
   - To meet requirements:
      - **Create user account to use AWS-Provided AWSAuthenticationPlugin with IAM.**

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

5. SA is developing a software on AWS that requires access to multiple AWS services, including EC2 instance.
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
