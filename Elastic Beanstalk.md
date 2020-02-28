## Elastic Beanstalk

1. Automatically scales your app up and down based on your applications specific need using easily adjustable Auto scaling settings.
2. With Elastic Beanstalk, your application can handle peaks in workload or traffic while minimizing your costs.
3. Automatically handles deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring, you just have to concentrate on your code.
4. Elastic Beanstalk is a AWS managed service that can do scalability. No auto scaling required.

Scenes:

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
