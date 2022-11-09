# AWS Note

## Services

- EC2 - Elastic Cloud Compute
  - Auto Scaling
  - Route 53 (DNS Service)
  - CloudWatch (Tracking & Logging)
  - CloudFront (CDN)
  - CloudFormation (Provision)
  - ELB - Elastic Load Balancing
  - Application Load Balancer
    - WAF
    - Protection & Request Tracing
    - Dynamic Ports
  - Security Group
    - built-in firewalls
    - Control access to instance
  - AZ  - Availability Zone
  - VPC - Virtual Private Cloud
    - isolate and expose resources inside VPC
    - security control
    - Multiple VPCs per account
    - lives within a Region
    - Subnets
    - Route tables
    - IGW - Internet Gateway
    - NAT Gateway
    - NACL - Network Access Control Lists
    - Sample
      - Region
        - VPC
          - Availability Zone A
            - Public Subnet A1 -> IGW
            - Private Subnet B1
- EBS - Elastic Block Storage
- S3  - Simple Storage Service
  - Storeing Application Assets
  - Static Web Hosting
  - Backup & Disaster Recovery
  - Staging area for Big Data
- SNS - Simple Notification Service
- RDS - Relational Database Services
- Lambda
- Beanstalk

## Architecture

### Security

- IAM - Identity and access management
- Detective controls
- Infrastructure protection
- Data protection
- Incident response

**Design Principles**

- Implement security at all layers
- Enable traceability
- Apply principle of least privilege
- Focus on securing your system
- Automate

---

### Reliability

- Recover from issues/failures
- Apply best practices in:
  - Foundations
  - Change management
  - Failure management
- Anticipate, respond and prevent failures

**Design Principles**

- Test recovery procedures
- Automatically recover
- Scale horizontally
- Stop guessing capacity
- Manage change in automation

---

### Performance efficiency

- Select customizable solutions
- Review to continually innovate
- Monitor AWS services
- Consider the trade-offs

**Design Principles**

- Democratize advanced technologies
- Go global in minutes
- Use a serverless architectures
- Experiment more often
- Have mechanical sympathy

---

### Cost optimization

- Use cost-effective resources
- Matching supply with demand
- Increase expenditure awareness
- Optimize over time

**Design Principles**

- Adopt a consumption model
- Measure overall effecieny
- Reduce spending on data center operations
- Analyze and attribute expenditure
- Use managed services

---

### Operational excellence

- Manage and automate changes
- Respond to events
- Define the standards

---

## Fault Tolerance

- Ability of a system to remain operational
- Built-in redundancy of an application's components

## High Availability

- Systems are generally functionality and accessible
- Downtime is minimized
- Minimal human intervention is required
- Minimal up-front financial investment
