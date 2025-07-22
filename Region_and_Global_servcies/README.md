
# 🌍 AWS Services – Region-Specific vs Global

This file outlines **which AWS services are region-specific** and **which are global**, with explanations.

---

## ✅ Region-Specific AWS Services

These services operate **within a specific AWS region**. Resources you create are tied to the selected region.

| Service Name       | Description |
|--------------------|-------------|
| **Amazon EC2**     | Instances are deployed in specific Availability Zones within a selected region. |
| **Amazon S3**      | Buckets and objects are stored in a chosen region, but data is replicated across AZs. |
| **Amazon RDS**     | Databases are hosted within a single region; Multi-AZ deployments increase availability. |
| **Elastic Load Balancer (ELB)** | Load balancers operate within one region and can span multiple AZs. |
| **AWS Lambda**     | Functions run in a specific region. |
| **Amazon VPC**     | Virtual networks created per region. Subnets map to AZs in that region. |
| **Amazon EBS**     | Volumes must reside in the same AZ as the instance they’re attached to. |
| **Amazon DynamoDB**| Tables are regional unless using Global Tables. |
| **CloudWatch**     | Metrics and logs are stored per region. |
| **Elastic Beanstalk** | Applications are launched in a region. |
| **AWS ECS / EKS**  | Clusters are created per region, nodes span multiple AZs. |
| **AWS WAF (Regional)** | Can be attached to ALBs/NLBs within a region. |

---

## 🌐 Global AWS Services

These services are **not tied to any one region** and operate globally across AWS.

| Service Name       | Description |
|--------------------|-------------|
| **Amazon Route 53**| Global DNS and routing service. |
| **AWS IAM**        | Global identity and permissions management across all regions. |
| **Amazon CloudFront**| Global CDN with edge locations worldwide. |
| **AWS WAF (with CloudFront)**| WAF integrated with CloudFront applies globally. |
| **AWS Shield**     | DDoS protection that works globally with CloudFront or Route 53. |

---

> 🔄 *Use region-specific services for proximity and latency; global services for centralized management and performance optimization.*
