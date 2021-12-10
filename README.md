# AWS Autoscaling with Launch Templates

Example from my udemy enroll course:
[Udemy: Terraform on AWS with SRE & IaC DevOps | Real-World 20 Demos](https://www.udemy.com/course/terraform-on-aws-with-sre-iac-devops-real-world-demos/)

![image](https://drive.google.com/uc?export=view&id=1tfBcN86vuW9elPuQVIea6jr9xFZQxzH-)

## Pre-requisite

- You need a Registered Domain in AWS Route53 to implement this usecase
- Create `private-key` folder
- Copy your AWS EC2 Key pair `terraform-key.pem` in `private-key` folder

## Populating Variables

The values for these variables should be placed into terraform.tfvars. Simply copy terraform.tfvars.example to terraform.tfvars and edit it with the proper values.

## Execute Terraform Commands

terraform init

terraform validate

terraform plan

terraform apply

## Verify via AWS Management Console

Observation:

1. Verify EC2 Instances created
2. Verify VPC
3. Verify Subnets
4. Verify IGW
5. Verify Public Route for Public Subnets
6. Verify no public route for private subnets
7. Verify NAT Gateway and Elastic IP for NAT Gateway
8. Verify NAT Gateway route for Private Subnets
9. Verify no public route or no NAT Gateway route to Database Subnets
10. Verify Subnets Security Group
11. Verify SSL Certificate (Certificate Manager)
12. Verify Route53 DNS Record
13. Verify Load Balancer
14. Verify Load Balancer Target Group - Health Checks
15. Verify Launch Templates (High Level)
16. Verify Autoscaling Group (High Level)
17. Verify Autoscaling Group Features In detail

- Details Tab
  - ASG Group Details
  - Launch Configuration
- Activity Tab
- Automatic Scaling
  - Target Tracking Scaling Policies (TTSP)
  - Scheduled Actions
- Instance Management
  - Instances
  - Lifecycle Hooks
- Monitoring
  - Autoscaling
  - EC2
- Instance Refresh Tab
18. Verify Tags

## Connect to Bastion EC2 Instance and Test

```t
# Connect to Bastion EC2 Instance from local desktop
ssh -i private-key/terraform-key.pem ec2-user@<PUBLIC_IP_FOR_BASTION_HOST>

# Curl Test for Bastion EC2 Instance to Private EC2 Instances
curl  http://<Private-Instance-App1-Private-IP>

# Connect to Private EC2 Instances App 1 from Bastion EC2 Instance
ssh -i /tmp/terraform-key.pem ec2-user@<Private-Instance-App1-Private-IP>
cd /var/www/html
ls -lrta
Observation: 
1) Should find index.html
2) Should find app1 folder
3) Should find app1/index.html file
4) Should find app1/metadata.html file
```

## Access and Test
```t
# App1 
https://asg-lt.domain.com
https://asg-lt.domain.com/app1/index.html
https://asg-lt.domain.com/app1/metadata.html
```

## Test Autoscaling using Postman
- [Download Postman client and Install](https://www.postman.com/downloads/)
- Create New Collection: terraform-on-aws
- Create new Request: asg
- URL: https://asg-lc.domain.com/app1/metadata.html
- Click on **RUN**, with 5000 requests
- Monitor ASG -> Activity Tab
- Monitor EC2 -> Instances - To see if new EC2 Instances getting created (Autoscaling working as expected)
- It might take 5 to 10 minutes to autoscale with new EC2 Instances

## Terraform Destroy

terraform destroy

## Clean-Up

rm -rf .terraform*

rm -rf terraform.tfstate*
