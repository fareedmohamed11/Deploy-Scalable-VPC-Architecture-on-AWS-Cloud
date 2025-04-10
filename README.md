# Deploy-Scalable-VPC-Architecture-on-AWS-Cloud
![ Deploy-Scalable-VPC-Architecture-on-AWS-Cloud](https://github.com/fareedmohamed11/Deploy-Scalable-VPC-Architecture-on-AWS-Cloud/blob/b07b1260178fcaff02e9212dcc21b992571c6eb9/68747470733a2f2f696d6775722e636f6d2f4158443530796c2e706e67.jpg)

## Table of Contents
1. [Goal](#goal)  
2. [Pre-Requisites](#pre-requisites)  
3. [Pre-Deployment](#pre-deployment)  
4. [VPC Deployment](#vpc-deployment)  
5. [Validation](#validation)  

---

## Goal
Deploy a Modular and Scalable Virtual Network Architecture with Amazon VPC.

---

## Pre-Requisites
- You must have an [AWS account](https://aws.amazon.com/) to create infrastructure resources on AWS cloud.  
- [Source Code](https://github.com/your-repo/source-code)  

---

## Pre-Deployment
Customize the application dependencies mentioned below on AWS EC2 instance and create the Golden AMI:  
- [AWS CLI](https://aws.amazon.com/cli/)  
- [Install Apache Web Server](https://httpd.apache.org/)  
- [Install Git](https://git-scm.com/)  
- [Cloudwatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)  
- Push custom memory metrics to Cloudwatch.  
- [AWS SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)  

---

## VPC Deployment
1. Build VPC network (192.168.0.0/16) for Bastion Host deployment as per the architecture shown above.  
2. Build VPC network (172.32.0.0/16) for deploying Highly Available and Auto Scalable application servers as per the architecture shown above.  
3. Create NAT Gateway in Public Subnet and update Private Subnet associated Route Table accordingly to route the default traffic to NAT for outbound internet connection.  
4. Create Transit Gateway and associate both VPCs to the Transit Gateway for private communication.  
5. Create Internet Gateway for each VPC and Public Subnet associated Route Table accordingly to route the default traffic to IGW for inbound/outbound internet connection.  
6. Create Cloudwatch Log Group with two Log Streams to store the VPC Flow Logs of both VPCs.  
7. Enable Flow Logs for both VPCs and push the Flow Logs to Cloudwatch Log Groups and store the logs in the respective Log Stream for each VPC.  
8. Create Security Group for bastion host allowing port 22 from public.  
9. Deploy Bastion Host EC2 instance in the Public Subnet with EIP associated.  
10. Create S3 Bucket to store application-specific configuration.  
11. Create Launch Configuration with below configuration:  
    - Golden AMI  
    - Instance Type – t2.micro  
    - Userdata to pull the code from Bitbucket Repository to document root folder of webserver and start the httpd service.  
    - IAM Role granting access to Session Manager and to S3 bucket created in the previous step to pull the configuration. (Do not grant S3 Full Access)  
    - Security Group allowing port 22 from Bastion Host and Port 80 from Public.  
12. Create Auto Scaling Group with Min: 2 Max: 4 with two Private Subnets associated to 1a and 1b zones.  
13. Create Target Group and associate it with ASG.  
14. Create Network Load Balancer in Public Subnet and add Target Group as target.  
15. Update Route53 hosted zone with CNAME record routing the traffic to NLB.  

---

## Validation
To validate the deployment:  
1. As DevOps Engineer login to Private Instances via Bastion Host.  
2. Login to AWS Session Manager and access the EC2 shell from console.  
3. Browse web application from public internet browser using domain name and verify that page loaded.  
