# 🚀 Terraform AWS Scalable Web App  
[![Terraform](https://img.shields.io/badge/Terraform-%3E%3D1.0-623CE4?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![AWS](https://img.shields.io/badge/AWS-%20Infra-orange?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Checks](https://img.shields.io/github/workflow/status/AdGamal/terraform-aws-scalable-webapp/CI/main?label=CI&logo=github)](https://github.com/AdGamal/terraform-aws-scalable-webapp/actions) 
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

A production-style AWS architecture deployed entirely with **one Terraform file**.  
Private subnets, internal ALB, Auto Scaling, NAT, IAM, and a bastion host — all stitched together in clean Terraform code.

![Architecture](diagrams/diagram.png)
---

## 🌐 What This Project Deploys

### **Network Layer**
- VPC (`10.0.0.0/16`)
- 2 Public subnets (bastion + NAT)
- 2 Private subnets (web servers)
- Internet Gateway  
- NAT Gateway for outbound traffic

### **Compute & Scaling**
- Auto Scaling Group (2–3 EC2 instances)
- Launch Configuration with HTTP server
- Private EC2 instances (no public IPs)
- Bastion host for secure SSH entry

### **Load Balancing**
- Internal Application Load Balancer  
- Target Group + HTTP Health Checks  
- Listener on port 80

### **Security**
- Bastion SG → SSH from your IP  
- ALB SG → HTTP from VPC  
- App SG → HTTP only from ALB  
- NAT → outbound only  
- Private workload fully isolated  

### **IAM & Storage**
- IAM Role for EC2  
- IAM Policy for S3 bucket access  
- Instance Profile  
- S3 bucket  

---

## 🧩 ASCII Architecture Diagram

                 ┌──────────────────────────────────────┐
                 │              Internet                 │
                 └───────────────────┬──────────────────┘
                                     │
                                     ▼
                          ┌──────────────────────┐
                          │   Bastion Host (22)  │
                          └──────────┬───────────┘
                                     │  SSH
                                     │
            ┌────────────────────────┼────────────────────────┐
            │                        │                        │
            ▼                        ▼                        ▼
  ┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
  │ Public Subnet A  │      │ Public Subnet B  │      │   Internet GW    │
  │ (Bastion)        │      │ (NAT Gateway)    │      │ (Outbound only)  │
  └────────┬─────────┘      └────────┬─────────┘      └────────┬─────────┘
           │                        │                         │
           │                        │                         │
           │                        ▼ NAT                     │
       ┌──────────────────────────────────────────────────────────┐
       │                      Private Subnets                     │
       │   (Web Servers inside Auto Scaling Group)                │
       │                                                          │
       │      ┌──────────────────────┐     ┌────────────────────┐ │
       │      │   Web Server 1       │     │   Web Server 2     │ │
       │      └──────────┬───────────┘     └──────────┬────────┘  │
       │                 HTTP (80)                     │          │
       └──────────────────────────┬────────────────────┼──────────┘
                                  │                    │
                                  ▼                    ▼
                     ┌──────────────────────────────────────────┐
                     │     Internal Application Load Balancer   │
                     │              (Listener: 80)              │
                     └──────────────────────────────────────────┘


---

## 📈 Terraform Graph (Conceptual)

VPC
├── Subnets
│ ├── Public A (Bastion)
│ ├── Public B (NAT)
│ ├── Private A (ASG)
│ └── Private B (ASG)
├── Internet Gateway
├── NAT Gateway
├── Route Tables
│ ├── Public RT → IGW
│ └── Private RT → NAT
├── Security Groups
│ ├── bastion_sg
│ ├── alb_sg
│ └── app_sg
├── ALB
│ └── Target Group
│ └── Listener
├── IAM Role + Instance Profile
└── Auto Scaling Group
└── Launch Configuration


---

## ⚙️ How to Deploy

### Initialize Terraform  
```bash
terraform init
```

Preview Infrastructure
```
terraform plan
```

Apply Changes
```
terraform apply
```

Destroy Infrastructure
```
terraform destroy
```

📌 Usage Examples
1 — SSH into Bastion
``` ssh -i my-key.pem ec2-user@$(terraform output -raw bastion_public_ip) ```

2 — SSH into Private EC2
``` ssh ec2-user@<private-ip> ```

3 — Test Web App
``` curl http://$(terraform output -raw alb_dns_name) ```

🔐 Security Model
Component	Allowed Traffic
Bastion SG	SSH from your IP
ALB SG	HTTP from VPC only
App SG	HTTP only from ALB SG
Private EC2	No public IP, outbound via NAT
NAT	Outbound internet for updates

📁 Project Structure
terraform-aws-scalable-webapp/
│── main.tf
│── README.md
└── diagram/
    └── diagram.png


🧠 What You Learn

-VPC design & subnet isolation
-Internal ALB architecture
-How NAT enables private-instance outbound
-Why IAM roles replace access keys
-How Auto Scaling integrates with ALB
-How Terraform links networking + compute + IAM

🏁 Next Enhancements

-Add HTTPS (ACM)
-Add RDS (private DB layer)
-Add AWS WAF
-Add CI/CD pipeline
-Add CloudWatch alarms

📜 License

MIT License — free to use, fork, improve.
