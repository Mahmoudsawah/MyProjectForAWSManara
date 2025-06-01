Scalable Web Application with ALB and Auto Scaling Architecture:

 EC2-based Description: Deploy a simple web application on AWS using EC2 instances,
 ensuring high availability and scalability with Elastic Load Balancing (ALB) and
 Auto Scaling Groups (ASG). The project demonstrates best practices for compute scalability,
 security, and cost optimization.
 
 Key AWS Services Used:
 EC2: Launch instances for the web app. 
Application Load Balancer (ALB): Distributes traffic across multiple instances.
 Auto Scaling Group (ASG): Ensures instances scale based on demand. 
Amazon RDS (Optional): Backend database (MySQL/PostgreSQL) with Multi-AZ. 
IAM: Role-based access to instances. 

CloudWatch & SNS: Monitor performance and send alerts. 
Learning Outcomes: Setting up secure and scalable EC2-based web applications. 
Implementing high availability using ALB and ASG.
 Optimizing costs and performance using Auto Scaling policies.

Step-by-Step Guide Using AWS Console

Step 1: Prepare a Simple Web App
Step 2: Create a Launch Template
      EC2 > Launch Templates > Create launch template.
      Security Group: Allow ports 22 (SSH) and 80 (HTTP).

Step 3: Create an Application Load Balancer (ALB)
     EC2 > Load Balancers > Create Load Balancer.
     Choose Application Load Balancer.
     Name: WebAppALB.
     Scheme: Internet-facing.
     Listeners: Port 80.
     VPC & Subnets:



Step 4: Create an Auto Scaling Group
      EC2 > Auto Scaling Groups > Create Auto Scaling Group.
      Select the launch template created earlier.
     Network: Use the same VPC and public subnets.
     Attach to existing Target Group (WebAppTG).
     Desired Capacity: 2 (change later if needed).
     Min: 1, Max: 3.
     Scaling Policies:
     Use Target Tracking Scaling.
     Metric: Average CPU Utilization.
     Target value: e.g., 50%.

Step 5: Add RDS Database
     RDS > Create Database.
    engine = MySQL/PostgreSQL.
    Enable Multi-AZ Deployment.
   Security Group: Allow MySQL/Postgres port (3306/5432) from EC2’s security group.



Step 6: Setup IAM Role (for CloudWatch)
     IAM > Roles > Create Role.
     Trusted entity: EC2.
     Attach these policies:
     AmazonEC2RoleforSSM
    CloudWatchAgentServerPolicy
   Attach to EC2 launch template (edit → add IAM role).

Step 7: Monitoring & Alerts
    CloudWatch > Alarms > Create Alarm.
    Select Metric: EC2 > CPU Utilization.
    Set threshold (e.g., > 70% for 2 datapoints).
    Create an SNS topic.
    Subscribe with  email for alerts.
