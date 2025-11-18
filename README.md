# terraform-aws-scalable-webapp

[![Terraform](https://img.shields.io/badge/Terraform-%3E%3D1.0-623CE4?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![AWS](https://img.shields.io/badge/AWS-%20Infra-orange?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![CI](https://img.shields.io/badge/CI--%20checks-blue)](#ci)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

A practical, production-minded Terraform project that provisions a highly-available web application stack on AWS:

**VPC, public & private subnets, NAT, internal ALB, Auto Scaling Group, Bastion host, IAM roles, and S3.**  
Single-file Terraform layout (`main.tf`) for clarity and reproducibility.

---

## Quick summary
- Multi-AZ VPC (public + private subnets)  
- Internal Application Load Balancer (ALB)  
- Auto Scaling Group for app servers (1–3 instances)  
- Bastion host for secure SSH access  
- NAT Gateway for outbound internet from private subnets  
- IAM roles for EC2 → S3 (no static keys)

---

## Quick start

**Prereqs**
- Terraform ≥ 1.0  
- AWS CLI configured (profile or env vars)  
- An AWS account  
- An SSH key pair (create in AWS or `ssh-keygen`)

**Steps**
```bash
git clone git@github.com:<YOUR_GITHUB_USERNAME>/terraform-aws-scalable-webapp.git
cd terraform-aws-scalable-webapp

# init & validate
terraform init
terraform validate
terraform plan

# when ready
terraform apply
