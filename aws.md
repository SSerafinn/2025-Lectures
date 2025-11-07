# Introduction to AWS (Amazon Web Services)

## Course Overview
This lecture provides a comprehensive introduction to Amazon Web Services (AWS), covering fundamental concepts, core services, and practical applications for beginners.

---

## Table of Contents
1. [What is AWS?](#what-is-aws)
2. [Why AWS?](#why-aws)
3. [AWS Global Infrastructure](#aws-global-infrastructure)
4. [Core AWS Services](#core-aws-services)
5. [AWS Pricing Model](#aws-pricing-model)
6. [Security and Compliance](#security-and-compliance)
7. [Getting Started with AWS](#getting-started-with-aws)
8. [Hands-On Example](#hands-on-example)
9. [Best Practices](#best-practices)
10. [Resources and Next Steps](#resources-and-next-steps)

---

## What is AWS?

Amazon Web Services (AWS) is a comprehensive cloud computing platform provided by Amazon. Launched in 2006, AWS offers over 200 fully featured services from data centers globally.

### Key Characteristics
- **On-Demand Computing Resources**: Access computing power, storage, and databases when you need them
- **Pay-As-You-Go Pricing**: Only pay for what you use, with no upfront costs
- **Scalability**: Scale resources up or down based on demand
- **Global Reach**: Deploy applications in multiple geographic regions worldwide

---

## Why AWS?

### Benefits of Using AWS

**Cost Savings**
- No upfront infrastructure investment
- Reduced operational costs
- Economies of scale passed to customers

**Flexibility and Agility**
- Deploy applications in minutes, not months
- Experiment without long-term commitments
- Choose from multiple programming languages and operating systems

**Reliability**
- 99.99% uptime SLA for many services
- Built-in redundancy and backup
- Disaster recovery capabilities

**Security**
- Enterprise-grade security infrastructure
- Compliance with major certifications (SOC, ISO, PCI DSS)
- Advanced encryption and access controls

**Innovation**
- Access to cutting-edge technologies (AI/ML, IoT, Quantum)
- Continuous service updates and new features
- Focus on application development, not infrastructure management

---

## AWS Global Infrastructure

### Components

**Regions**
- Geographic areas containing multiple data centers
- Currently 33+ regions worldwide
- Each region is completely independent
- Example: us-east-1 (N. Virginia), eu-west-1 (Ireland)

**Availability Zones (AZs)**
- One or more discrete data centers within a region
- Each region has multiple AZs (typically 3 or more)
- Connected through low-latency links
- Provides fault tolerance and high availability

**Edge Locations**
- Content Delivery Network (CDN) endpoints
- 400+ edge locations globally
- Used by Amazon CloudFront for content caching
- Reduces latency for end users

---

## Core AWS Services

### Compute Services

**Amazon EC2 (Elastic Compute Cloud)**
- Virtual servers in the cloud
- Choose instance types based on CPU, memory, storage needs
- Pay only for what you use
- Use cases: Web hosting, application servers, batch processing

**AWS Lambda**
- Serverless computing service
- Run code without provisioning servers
- Automatic scaling
- Pay only for compute time consumed
- Use cases: Event-driven applications, microservices, data processing

**Amazon ECS/EKS**
- Container orchestration services
- ECS: Amazon's container service
- EKS: Managed Kubernetes service

### Storage Services

**Amazon S3 (Simple Storage Service)**
- Object storage service
- 99.999999999% (11 9's) durability
- Unlimited storage capacity
- Use cases: Backup, static website hosting, data lakes, content distribution

**Amazon EBS (Elastic Block Store)**
- Block storage for EC2 instances
- Persistent storage that survives instance termination
- Multiple volume types for different performance needs
- Use cases: Database storage, application data

**Amazon EFS (Elastic File System)**
- Managed file storage service
- Scalable, elastic file system for Linux workloads
- Shared access across multiple EC2 instances

### Database Services

**Amazon RDS (Relational Database Service)**
- Managed relational database service
- Supports MySQL, PostgreSQL, MariaDB, Oracle, SQL Server
- Automated backups, patching, and scaling
- Use cases: Web applications, enterprise applications

**Amazon DynamoDB**
- Fully managed NoSQL database
- Single-digit millisecond latency
- Automatic scaling
- Use cases: Mobile apps, gaming, IoT

**Amazon Aurora**
- MySQL and PostgreSQL-compatible relational database
- 5x faster than standard MySQL, 3x faster than PostgreSQL
- Fully managed with automated backups

### Networking Services

**Amazon VPC (Virtual Private Cloud)**
- Isolated cloud network environment
- Complete control over IP addresses, subnets, route tables
- Security groups and network ACLs for security
- Use cases: Secure application hosting, hybrid cloud architectures

**Amazon Route 53**
- Scalable DNS web service
- Domain registration and routing
- Health checking and monitoring

**AWS CloudFront**
- Content Delivery Network (CDN)
- Distribute content globally with low latency
- Integration with other AWS services

---

## AWS Pricing Model

### Core Principles

**Pay-As-You-Go**
- No upfront costs or long-term commitments
- Pay only for services consumed
- Stop paying when you stop using

**Save When You Reserve**
- Reserved Instances: Up to 75% savings with 1-3 year commitments
- Savings Plans: Flexible pricing model for compute usage

**Pay Less by Using More**
- Volume-based discounts
- Tiered pricing for data transfer and storage

### Free Tier

AWS offers a free tier for new customers:
- **12 months free**: Limited usage of popular services (EC2, S3, RDS)
- **Always free**: Lambda (1M requests/month), DynamoDB (25GB storage)
- **Trials**: Short-term free trials for other services

---

## Security and Compliance

### Shared Responsibility Model

**AWS Responsibility (Security OF the Cloud)**
- Physical security of data centers
- Hardware and infrastructure
- Virtualization layer
- Network infrastructure

**Customer Responsibility (Security IN the Cloud)**
- Data encryption
- Identity and access management
- Application security
- Operating system patching
- Network configuration

### Key Security Services

**AWS IAM (Identity and Access Management)**
- Control access to AWS services and resources
- Create users, groups, roles, and policies
- Multi-factor authentication (MFA)
- Best practice: Follow principle of least privilege

**AWS KMS (Key Management Service)**
- Managed encryption key creation and control
- Integration with other AWS services
- Audit key usage with CloudTrail

**AWS Shield**
- DDoS protection
- Standard (free) and Advanced tiers

---

## Getting Started with AWS

### Step-by-Step Guide

**1. Create an AWS Account**
```
- Visit aws.amazon.com
- Click "Create an AWS Account"
- Provide email, password, and account name
- Enter payment information (required, but free tier available)
- Verify identity via phone
```

**2. Set Up Security**
```
- Enable MFA on root account
- Create IAM users for day-to-day activities
- Never use root account for regular tasks
- Set up billing alerts
```

**3. Choose Your Region**
```
- Select region closest to your users
- Consider data sovereignty requirements
- Check service availability in region
```

**4. Explore the Console**
```
- Familiarize yourself with AWS Management Console
- Use search to find services
- Bookmark frequently used services
```

---

## Hands-On Example

### Launch a Simple Web Server on EC2

**Prerequisites**
- AWS account created
- IAM user with EC2 permissions

**Steps**

1. **Navigate to EC2 Dashboard**
   - Open AWS Console
   - Search for "EC2" and select the service

2. **Launch Instance**
   - Click "Launch Instance"
   - Name your instance (e.g., "my-web-server")

3. **Choose Amazon Machine Image (AMI)**
   - Select "Amazon Linux 2023 AMI" (free tier eligible)

4. **Choose Instance Type**
   - Select "t2.micro" (free tier eligible)
   - 1 vCPU, 1 GB memory

5. **Configure Key Pair**
   - Create new key pair or use existing
   - Download .pem file (keep it safe!)

6. **Configure Security Group**
   - Allow SSH (port 22) from your IP
   - Allow HTTP (port 80) from anywhere (0.0.0.0/0)

7. **Launch Instance**
   - Review settings and click "Launch Instance"
   - Wait for instance to be in "running" state

8. **Connect to Instance**
   ```bash
   chmod 400 your-key-pair.pem
   ssh -i "your-key-pair.pem" ec2-user@<public-ip>
   ```

9. **Install Web Server**
   ```bash
   sudo yum update -y
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   echo "<h1>Hello from AWS!</h1>" | sudo tee /var/www/html/index.html
   ```

10. **Test**
    - Open browser and navigate to instance's public IP
    - You should see "Hello from AWS!"

---

## Best Practices

### General Recommendations

**Design for Failure**
- Assume everything fails and design accordingly
- Use multiple Availability Zones
- Implement auto-scaling
- Regular backups and disaster recovery planning

**Implement Security at Every Layer**
- Use IAM roles instead of access keys when possible
- Enable CloudTrail for audit logging
- Encrypt data at rest and in transit
- Regular security audits

**Optimize for Cost**
- Right-size instances (don't over-provision)
- Use Reserved Instances for predictable workloads
- Delete unused resources
- Set up billing alerts and budgets
- Use AWS Cost Explorer

**Automate Everything**
- Use Infrastructure as Code (CloudFormation, Terraform)
- Automate deployments with CI/CD
- Use AWS Systems Manager for patch management

**Monitor and Log**
- Enable CloudWatch monitoring
- Set up alarms for critical metrics
- Centralize logs with CloudWatch Logs
- Use AWS X-Ray for application tracing

---

## Resources and Next Steps

### Official AWS Resources

**Documentation**
- AWS Documentation: https://docs.aws.amazon.com
- AWS Well-Architected Framework: Best practices for cloud architecture
- AWS Whitepapers: In-depth technical guides

**Training and Certification**
- AWS Skill Builder: Free digital training
- AWS Training: Instructor-led and self-paced courses
- AWS Certifications: 
  - Cloud Practitioner (foundational)
  - Solutions Architect Associate
  - Developer Associate

**Community and Support**
- AWS Forums: Community discussion boards
- AWS re:Post: Q&A service
- AWS Support Plans: Basic (free) to Enterprise

### Learning Path Recommendations

**Beginner**
1. Complete AWS Cloud Practitioner training
2. Explore core services hands-on (EC2, S3, RDS)
3. Build simple projects (static website, web application)

**Intermediate**
1. Study Solutions Architect Associate content
2. Learn Infrastructure as Code (CloudFormation)
3. Implement multi-tier applications
4. Explore serverless architectures

**Advanced**
1. Pursue specialty certifications
2. Design complex, scalable architectures
3. Implement DevOps practices
4. Contribute to AWS community

---

## Summary

AWS provides a comprehensive cloud computing platform that enables organizations to:
- Build scalable and reliable applications
- Reduce infrastructure costs
- Innovate faster
- Focus on business value rather than infrastructure management

Key takeaways:
- AWS offers 200+ services across compute, storage, database, and more
- Pay-as-you-go pricing with free tier for beginners
- Global infrastructure with high availability and fault tolerance
- Security is a shared responsibility
- Start small, experiment, and scale as needed

---

## Questions?

**Common Questions**

*Q: How much does AWS cost?*
A: It depends on usage. Start with the free tier and use the AWS Pricing Calculator to estimate costs.

*Q: Is AWS difficult to learn?*
A: AWS has a learning curve, but abundant resources are available. Start with fundamentals and build gradually.

*Q: Which certification should I pursue first?*
A: Start with AWS Certified Cloud Practitioner for foundational knowledge.

*Q: Can I use AWS for personal projects?*
A: Yes! The free tier is perfect for learning and personal projects.

---

## Additional Practice Exercises

1. **Create an S3 bucket and host a static website**
2. **Set up a VPC with public and private subnets**
3. **Deploy a database with RDS and connect it to an EC2 instance**
4. **Create a Lambda function triggered by S3 events**
5. **Implement auto-scaling for an EC2-based application**

---

*Last Updated: November 2025*

*This lecture material is for educational purposes. AWS services and features are continuously updated. Always refer to official AWS documentation for the most current information.*
