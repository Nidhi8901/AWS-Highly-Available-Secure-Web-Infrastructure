# ☁️ AWS Scalable & Secure Web Infrastructure

A hands-on AWS cloud infrastructure project demonstrating **high availability, scalability, secure networking, load balancing, content delivery, and responsible cloud resource management**.

The project uses Amazon VPC, EC2, Auto Scaling, Application Load Balancer, CloudFront, IAM, Security Groups, public/private subnet design, and NAT-based outbound connectivity.

---

## 📌 Project Overview

This project demonstrates how a production-style web workload can be deployed on AWS using multiple infrastructure layers.

The architecture was designed to:

- distribute incoming traffic efficiently
- scale EC2 capacity based on demand
- improve global content delivery using CloudFront
- isolate resources using VPC networking
- monitor backend instance health
- reduce direct exposure of compute resources
- support high availability across Availability Zones
- clean up cloud resources after validation to avoid unnecessary charges

---

## 🏗 Architecture

![AWS Architecture](architecture/Architecture-diagram.jpeg)

### Request Flow

```text
User
  │
  ▼
Amazon CloudFront
  │
  ▼
Application Load Balancer
  │
  ▼
Target Group
  │
  ▼
EC2 Instances
  │
  ▼
Application
```

The infrastructure is deployed inside an Amazon VPC with subnet segmentation, security groups, load balancing, and scaling components.

---

## 🧰 AWS Services & Technologies

| Service | Purpose |
|---|---|
| Amazon VPC | Provides isolated AWS networking |
| Public & Private Subnets | Separates internet-facing and internal resources |
| Internet Gateway | Provides internet connectivity to public resources |
| NAT Gateway | Provides outbound internet access for private resources |
| Amazon EC2 | Hosts the application workload |
| Auto Scaling Group | Automatically adjusts EC2 capacity |
| Application Load Balancer | Distributes traffic across healthy EC2 instances |
| Target Groups | Routes requests and performs health checks |
| Amazon CloudFront | Provides edge caching and global content delivery |
| Security Groups | Controls inbound and outbound network traffic |
| IAM | Provides controlled AWS access and permissions |

---

## 🔄 Architecture Flow

### 1. User Request

Users access the application through the CloudFront distribution.

```text
User → CloudFront
```

CloudFront acts as the entry point and helps deliver content using AWS edge locations.

### 2. Load Balancing

CloudFront forwards application traffic to the Application Load Balancer.

```text
CloudFront → ALB
```

The ALB distributes incoming requests across registered backend instances.

### 3. Target Group

The Application Load Balancer routes requests to healthy EC2 instances through a target group.

```text
ALB → Target Group → EC2
```

Health checks ensure that traffic is sent only to healthy instances.

### 4. Auto Scaling

EC2 instances are managed using an Auto Scaling design so application capacity can grow or shrink depending on workload requirements.

### 5. Network Security

The infrastructure is deployed inside an Amazon VPC with subnet separation and security-group rules controlling access between components.

NAT-based outbound connectivity was configured during the implementation and later removed during project cleanup.

---

## 🌍 CloudFront Distribution

Amazon CloudFront was configured in front of the application infrastructure to improve content delivery and provide a global entry point.

### Distribution

![CloudFront Distribution](screenshots/cloudfront/cloudfront-distribution.jpeg)

### CloudFront Application Output

![CloudFront Output](screenshots/cloudfront/cloudfront-output.jpeg)

---

## ⚖️ Application Load Balancer

An Application Load Balancer distributes incoming application traffic between available backend targets.

![Application Load Balancer](screenshots/load-balancer/alb.jpeg)

### Application Output Through ALB

![ALB Output](screenshots/load-balancer/alb-output.jpeg)

---

## ❤️ Target Group Health

Target groups were configured to monitor EC2 instances and route traffic only to healthy application targets.

![Healthy Target Group](screenshots/target-group/target-group-healthy.jpeg)

This helps improve availability because unhealthy targets can be removed from traffic routing automatically.

---

## 🖥 EC2 Compute Layer

Amazon EC2 instances provide the compute layer for the application.

![Running EC2 Instances](screenshots/ec2/running-ec2-instances.jpeg)

The architecture was designed so multiple instances can operate behind the load balancer instead of relying on a single server.

---

## 🌐 VPC & Subnet Configuration

The infrastructure was deployed inside a dedicated Amazon VPC.

### Public Subnet 1

![Public Subnet 1](screenshots/vpc-subnets/vpc-subnets-public-subnet-1.jpeg)

### Public Subnet 2

![Public Subnet 2](screenshots/vpc-subnets/vpc-subnets-public-subnet-2.jpeg)

Using multiple subnets supports an architecture that can span multiple Availability Zones.

---

## 📈 Scalability

The architecture incorporates multiple AWS scalability concepts:

- Application Load Balancer for traffic distribution
- Auto Scaling for dynamic compute capacity
- multiple EC2 instances
- multiple subnet design
- CloudFront edge delivery
- target-group health monitoring

This reduces dependency on a single compute instance and makes the architecture better suited for changing workloads.

---

## 🔐 Security Design

Security was considered at several layers of the infrastructure.

### Network Isolation

Resources were deployed within an Amazon VPC with subnet separation.

### Security Groups

Security groups control which traffic is allowed between AWS resources.

### IAM

IAM roles and policies were used to control AWS permissions.

### Controlled Application Access

Application traffic is routed through the load-balancing layer instead of exposing each backend instance as the primary application endpoint.

---

## ✅ Health Checks

Application Load Balancer target-group health checks continuously verify backend instance availability.

```text
ALB
 │
 ▼
Target Group Health Check
 │
 ├── Healthy → Receive traffic
 │
 └── Unhealthy → Removed from routing
```

This improves service reliability and helps prevent traffic from being sent to unhealthy application instances.

---

## 📂 Repository Structure

```text
AWS-3-Tier-Scalable-Secure-Infrastructure-using-ALB-Auto-Scaling-and-CloudFront/
│
├── README.md
│
├── architecture/
│   └── Architecture-diagram.jpeg
│
├── notes/
│   └── cleanup-steps.md
│
└── screenshots/
    │
    ├── cloudfront/
    │   ├── cloudfront-distribution.jpeg
    │   └── cloudfront-output.jpeg
    │
    ├── ec2/
    │   └── running-ec2-instances.jpeg
    │
    ├── load-balancer/
    │   ├── alb.jpeg
    │   └── alb-output.jpeg
    │
    ├── target-group/
    │   └── target-group-healthy.jpeg
    │
    └── vpc-subnets/
        ├── vpc-subnets-public-subnet-1.jpeg
        └── vpc-subnets-public-subnet-2.jpeg
```

---

## 🚀 Key Features

- AWS VPC-based network architecture
- multi-subnet deployment design
- Application Load Balancer
- EC2 compute infrastructure
- Auto Scaling architecture
- CloudFront content delivery
- target-group health monitoring
- security-group-based network control
- IAM-based access management
- architecture documentation
- AWS resource cleanup documentation

---

## 💼 Real-World Applications

A similar architecture pattern can be used for:

- web applications
- e-commerce platforms
- business websites
- API backends
- internal enterprise applications
- horizontally scalable application workloads

---

## 🧹 AWS Resource Cleanup

All AWS resources created for this project were intentionally removed after implementation and validation to avoid unnecessary cloud charges.

Resources cleaned up included:

```text
CloudFront Distribution
Application Load Balancer
Target Groups
Auto Scaling Group
EC2 Instances
NAT Gateway
Elastic IPs
Security Groups
Subnets
VPC
```

Detailed cleanup information is available here:

➡️ [AWS Resource Cleanup Steps](notes/cleanup-steps.md)

This demonstrates awareness of AWS cost management and responsible cloud resource usage.

---

## 🧠 Skills Demonstrated

Through this project I gained practical experience with:

- AWS networking
- VPC architecture
- subnet design
- EC2 administration
- Application Load Balancing
- Auto Scaling concepts
- CloudFront CDN
- AWS health checks
- security groups
- IAM
- scalable infrastructure design
- cloud cost awareness
- architecture documentation

---

## 📸 Deployment Evidence

The repository contains screenshots documenting the implemented AWS infrastructure, including:

```text
✓ CloudFront distribution
✓ CloudFront application output
✓ Application Load Balancer
✓ ALB application output
✓ Healthy Target Group
✓ Running EC2 instances
✓ VPC / Public Subnets
✓ Architecture diagram
```

---

## ⚠️ Infrastructure Evidence Note

The AWS resources were created and validated during the project and were intentionally deleted afterward to prevent ongoing AWS charges.

The screenshots, architecture diagram, and cleanup documentation in this repository provide evidence of the implementation.

The current repository primarily documents the **networking, edge-delivery, load-balancing, and compute layers**. Database-layer configuration artifacts are not included in the repository.

---

## 👩‍💻 Author

**Nidhi Kumari**

GitHub: [Nidhi8901](https://github.com/Nidhi8901)

LinkedIn: [Nidhi Kumari](https://www.linkedin.com/in/nidhi-kumari-ba2a1a361)

---

⭐ If you found this project useful, consider starring the repository.
