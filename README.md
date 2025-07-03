# AWS Java Application Deployment Project

This project demonstrates the deployment of a scalable Java web application on AWS using a complete DevOps approach. The infrastructure is built with high availability, auto scaling, and DNS management in mind.

## 🛠 Tools & Services Used

- **Amazon EC2**: Hosts the Java application using Apache Tomcat.
- **Auto Scaling Group**: Ensures high availability by automatically adjusting the number of EC2 instances.
- **Application Load Balancer (ALB)**: Distributes incoming traffic across EC2 instances.
- **Target Group**: Defines the set of instances that receive traffic from the ALB.
- **Amazon Route 53**: Handles public and private DNS routing.
- **S3 Bucket**: Stores static assets (if needed).
- **Private DNS Zones**: Manages internal resolution for services (e.g., database, cache, MQ).

## 🔧 Architecture Overview

1. **Users** access the application through a domain registered in **GoDaddy**, mapped to the **ALB endpoint** using **Route 53**.
2. **ALB** receives incoming HTTPS requests and forwards them over **port 8080** to the **target group** of EC2 instances.
3. **EC2 Instances** run the Java application inside a Tomcat server. These instances are managed by an **Auto Scaling Group** for elasticity.
4. The application communicates with:
   - **MySQL** for database operations
   - **Memcached** for caching
   - **RabbitMQ** for messaging
   These services reside in a private security group with internal DNS names (e.g., `db01`, `mc01`, `rmq01`) resolved via **Route 53 private hosted zones**.
5. Static files or assets (if required) are served from an **S3 bucket**.

##  Security Groups

- **ALB Security Group**: Allows HTTPS (port 443) traffic from the internet.
- **App Security Group**: Accepts traffic only from the ALB on port 8080.
- **Database/Messaging Security Group**: Accepts traffic from the application tier only, based on strict IP and port rules.

##  Auto Scaling

Auto Scaling adjusts the number of EC2 instances based on:
- CPU utilization
- Network I/O
- Custom CloudWatch metrics

## 🧠 Internal DNS Mapping

Private DNS records are configured in Route 53:
- `db01` ➜ MySQL instance IP
- `mc01` ➜ Memcached instance IP
- `rmq01` ➜ RabbitMQ instance IP

These mappings allow the application tier to access backend services using consistent DNS names.

## 📁 Repository Structure

aws-java-app/
├── scripts/ # Automation and setup scripts
├── app/ # Java/Tomcat application source
├── terraform/ or cloudformation/ # (optional) Infrastructure as Code files
├── docs/ # Architecture diagrams and documentation
└── README.md # Project overview
