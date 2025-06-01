# Scalable Web Application with ALB and Auto Scaling Architecture

This project demonstrates how to deploy a **scalable and highly available web application** on AWS using **EC2 instances**, **Application Load Balancer (ALB)**, and **Auto Scaling Groups (ASG)**. It follows best practices for **compute scalability**, **security**, and **cost optimization**.

---

## 🧰 Key AWS Services Used

- **Amazon EC2** – Launch instances for the web application.
- **Application Load Balancer (ALB)** – Distribute traffic across multiple EC2 instances.
- **Auto Scaling Group (ASG)** – Automatically scale EC2 instances based on demand.
- **Amazon RDS** *(Optional)* – Backend database (MySQL/PostgreSQL) with Multi-AZ for high availability.
- **IAM** – Role-based access to AWS services from EC2.
- **Amazon CloudWatch & SNS** – Monitor performance and send alerts.

---

## 🎯 Learning Outcomes

- Deploy secure and scalable EC2-based web applications.
- Implement high availability using ALB and ASG.
- Optimize cost and performance with Auto Scaling policies.
- Monitor infrastructure and configure alerting with CloudWatch and SNS.

---

## 🛠 Step-by-Step Guide Using AWS Console

### Step 1: Prepare a Simple Web App

- Package a lightweight web application (e.g., simple HTML or Node.js/Flask app).
- Ensure it runs on port 80.

---

### Step 2: Create a Launch Template

**Console Path:**  
`EC2 > Launch Templates > Create launch template`

- Configure with AMI, instance type, and user data to run the web app.
- Create a **Security Group**:
  - Allow port **22 (SSH)**.
  - Allow port **80 (HTTP)**.

---

### Step 3: Create an Application Load Balancer (ALB)

**Console Path:**  
`EC2 > Load Balancers > Create Load Balancer`

- Type: `Application Load Balancer`
- Name: `WebAppALB`
- Scheme: `Internet-facing`
- Listener: `Port 80`
- VPC & Subnets: Use **public subnets** in **at least two AZs**

---

### Step 4: Create an Auto Scaling Group

**Console Path:**  
`EC2 > Auto Scaling Groups > Create Auto Scaling Group`

- Choose the launch template created earlier.
- VPC & Subnets: Same as ALB (public subnets)
- Attach to existing **Target Group** (`WebAppTG`)
- Desired Capacity: `2`
- Minimum: `1`, Maximum: `3`
- Scaling Policies:
  - **Type:** Target Tracking
  - **Metric:** Average CPU Utilization
  - **Target Value:** `50%`

---

### Step 5: Add RDS Database *(Optional)*

**Console Path:**  
`RDS > Create Database`

- Engine: `MySQL` or `PostgreSQL`
- **Enable Multi-AZ deployment**
- Create a **Security Group**:
  - Allow port `3306` (MySQL) or `5432` (PostgreSQL) from EC2 Security Group.

---

### Step 6: Setup IAM Role for Monitoring

**Console Path:**  
`IAM > Roles > Create Role`

- Trusted Entity: `EC2`
- Attach Policies:
  - `AmazonEC2RoleforSSM`
  - `CloudWatchAgentServerPolicy`
- Attach the role to EC2 Launch Template:
  - Edit launch template → Add IAM Role

---

### Step 7: Setup Monitoring & Alerts

**Console Path:**  
`CloudWatch > Alarms > Create Alarm`

- Metric: `EC2 > CPU Utilization`
- Condition: CPU > `70%` for 2 datapoints
- Action:
  - Create **SNS Topic**
  - Add **email subscription** to receive alerts

---


