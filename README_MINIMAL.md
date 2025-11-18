# Terraform AWS Scalable Web App

A clean, single-file Terraform setup that deploys:

- VPC (10.0.0.0/16)
- 2 Public + 2 Private Subnets
- Bastion Host
- NAT Gateway
- Internal Application Load Balancer
- Auto Scaling Group (EC2)
- IAM Role + S3 Access
- Full routing & security groups

Perfect as a reference architecture for junior → mid DevOps interviews.

## Deploy
terraform init
terraform plan
terraform apply


## Destroy


terraform destroy


## Diagram
![Architecture](.diagrams/diagram.png)
