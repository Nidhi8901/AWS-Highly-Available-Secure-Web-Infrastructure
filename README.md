<div align="center">

# Highly Available & Secure Web Infrastructure on AWS

### CloudFront · Application Load Balancer · EC2 Auto Scaling · VPC · IAM

A hands-on AWS infrastructure project focused on **traffic distribution, scalable compute, controlled network access, and reliable application delivery**.

![AWS](https://img.shields.io/badge/Cloud-AWS-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![Infrastructure](https://img.shields.io/badge/Focus-Cloud%20Infrastructure-1976D2?style=flat-square)
![Project Status](https://img.shields.io/badge/Status-Completed-238636?style=flat-square)
![Deployment](https://img.shields.io/badge/Deployment-Demonstrated%20%26%20Cleaned%20Up-555?style=flat-square)

**[Architecture](#architecture--request-flow) · [Implementation](#what-i-built) · [Technical Decisions](#engineering-decisions) · [Evidence](#deployment-evidence) · [Validation](#validation--project-scope)**

</div>

---

## Project overview

Instead of exposing a single EC2 instance as a website endpoint, this project uses an **edge-to-application delivery path**: Amazon CloudFront accepts the entry request, an Application Load Balancer (ALB) routes it to healthy registered instances, and an EC2 Auto Scaling Group manages the compute-capacity design. The surrounding VPC, subnet setup, security groups, and IAM define network and access boundaries.

The AWS environment was **implemented, tested for the behaviors shown in the screenshots, and then deleted to avoid ongoing charges**. This is a documented infrastructure implementation—not a currently hosted live website or a one-command deployment template.

| Design goal | AWS implementation |
| :--- | :--- |
| Distribute traffic | Application Load Balancer and target groups |
| Support changing compute demand | EC2 Auto Scaling architecture |
| Deliver through an edge endpoint | Amazon CloudFront distribution |
| Control network and resource access | VPC, subnets, security groups, IAM |
| Check application target health | ALB target-group health checks |
| Keep project costs controlled | Resource cleanup after validation |

## Architecture & request flow

![Architecture diagram — CloudFront, ALB, target group, EC2 and VPC](architecture/Architecture-diagram.jpeg)

```text
                     Web user / browser
                            |
                            v
                    Amazon CloudFront
                            |
                            v
                  Application Load Balancer
                            |
                            v
                 Target group + health checks
                            |
                            v
                    EC2 application instances
                            ^
                            |
                     EC2 Auto Scaling

             AWS VPC · subnets · security groups
                   IAM access controls
```

**Request path:** `Browser → CloudFront → ALB → Healthy target → EC2 application`

The load balancer routes traffic only to healthy registered targets. The Auto Scaling Group provides a framework for managing EC2 capacity. Subnets and security controls provide the networking environment around these services.

## What I built

### 1. Compute, load balancing, and target health

- Deployed Amazon EC2 instances for the application workload.
- Configured an **Application Load Balancer** with a target group to route HTTP application traffic to backend instances.
- Inspected target-group health and validated that the application was reachable through the ALB endpoint.
- Configured an **EC2 Auto Scaling** architecture to support capacity management.

### 2. Edge delivery with CloudFront

- Set up a CloudFront distribution in front of the application delivery path.
- Verified application output through CloudFront and through the ALB.
- Kept the entry-point architecture distinct from the underlying compute instances.

### 3. VPC networking and access control

- Worked with an Amazon VPC, public subnets, security groups, and internet routing components.
- Configured NAT-based outbound connectivity during the project setup.
- Applied IAM roles/policies and network access rules to control infrastructure access.

### 4. Documentation and cloud operations

- Recorded the infrastructure in an architecture diagram and AWS console screenshots.
- Verified the implemented request flow and documented the relevant AWS resource states.
- Removed deployed resources once the validation work was complete to prevent unnecessary ongoing charges.

## Engineering decisions

| Decision | Why it matters | Evidence |
| :--- | :--- | :--- |
| Put an ALB in front of EC2 | Decouples the application entry point from individual instances and enables health-aware routing. | ALB output + target-group screenshots |
| Use EC2 Auto Scaling | Establishes a capacity-management design rather than relying on one fixed instance. | Architecture and implementation documentation |
| Add CloudFront | Provides an edge-facing distribution for application access. | Distribution configuration + application-output screenshots |
| Use VPC and security groups | Separates networking concerns and defines allowed traffic between resources. | VPC/subnet screenshots and documented configuration |
| Clean up the environment | Prevents continued billing after a learning/demo deployment. | [`notes/cleanup-steps.md`](notes/cleanup-steps.md) |

## Deployment evidence

The screenshots are retained in the repository so the implemented setup can be inspected even after the AWS resources were deleted.

### CloudFront — edge distribution and application response

| CloudFront distribution | Application accessed through CloudFront |
| :---: | :---: |
| ![CloudFront distribution configuration](screenshots/cloudfront/cloudfront-distribution.jpeg) | ![Application output through CloudFront](screenshots/cloudfront/cloudfront-output.jpeg) |

### Application Load Balancer — configuration and response

| ALB resource | Application accessed through ALB |
| :---: | :---: |
| ![Application Load Balancer](screenshots/load-balancer/alb.jpeg) | ![Application output through ALB](screenshots/load-balancer/alb-output.jpeg) |

<details>
<summary><strong>More implementation screenshots — health checks, EC2 and VPC</strong></summary>

#### Target-group health

![Healthy ALB target group](screenshots/target-group/target-group-healthy.jpeg)

#### Running EC2 instances

![EC2 instances running](screenshots/ec2/running-ec2-instances.jpeg)

#### VPC / public subnet configuration

| Public subnet 1 | Public subnet 2 |
| :---: | :---: |
| ![Public subnet 1](screenshots/vpc-subnets/vpc-subnets-public-subnet-1.jpeg) | ![Public subnet 2](screenshots/vpc-subnets/vpc-subnets-public-subnet-2.jpeg) |

</details>

## Validation & project scope

| What was verified or documented | Status |
| :--- | :--- |
| Application output through the ALB | Documented with screenshot |
| CloudFront distribution and application output | Documented with screenshots |
| Healthy registered target-group state | Documented with screenshot |
| Running EC2 instances and public subnets | Documented with screenshots |
| AWS resource cleanup | Documented in repository notes |
| Simulated Availability Zone failure / zero-downtime recovery | **Not documented as tested** |
| Measured Auto Scaling recovery time or load-test results | **Not documented as tested** |
| RDS or a separately deployed database layer | **Not part of the documented implementation** |

**High availability** here describes the infrastructure *design approach*. The repository does not claim a measured outage-recovery result, an RDS Multi-AZ implementation, or a completed three-tier application.

## Implementation outline

This is a **console-configured, screenshot-documented project**, not an IaC deployment kit. The following sequence explains how to reproduce the *general architecture*; it is not an automated script or a claim of additional validation steps.

1. Configure a VPC, subnets, routing, security groups, and the needed IAM permissions.
2. Prepare EC2 compute instances and an application listener.
3. Create a target group, attach an ALB, and configure the health checks.
4. Configure an EC2 Auto Scaling Group for the compute layer.
5. Create a CloudFront distribution pointing to the application entry point.
6. Confirm application access via the ALB and CloudFront, and inspect target-group health.
7. Capture evidence and remove the temporary AWS resources when no longer needed.

## Repository layout

```text
.
├── README.md
├── architecture/
│   └── Architecture-diagram.jpeg
├── screenshots/
│   ├── cloudfront/
│   ├── ec2/
│   ├── load-balancer/
│   ├── target-group/
│   └── vpc-subnets/
└── notes/
    └── cleanup-steps.md
```

## Cost management & cleanup

Because ALB, CloudFront, NAT Gateway, EC2, and related AWS services can incur charges, the resources were intentionally removed after validation. The cleanup record covers CloudFront, the ALB and target group, the Auto Scaling Group, EC2, NAT Gateway, associated network resources, and supporting infrastructure.

**[Read the AWS resource cleanup notes →](notes/cleanup-steps.md)**

## Potential next improvements

These are **future enhancements**, not claims about the present project:

- Terraform or CloudFormation templates for a repeatable deployment.
- Recorded load testing and Auto Scaling behavior with actual metrics.
- Controlled EC2/AZ failure testing with recovery-time measurements.
- A separately documented application/database tier, if the project expands.
- CloudWatch alarms and more complete operational dashboards.

---

<div align="center">

**Nidhi Kumari** · [GitHub](https://github.com/Nidhi8901) · [Portfolio](https://nidhikumari-portfolio.netlify.app)

</div>
