# Terraform snippet for deploying Highly available ASG in private subnet behind ALB

# Tasks:

1. Deploy a VPC with custom CIDR
2. Create 2 public subnets and 2 private subnets with custom CIDRs — where private subnets are used for compute instances running webservers & public subnets are used by application load balancers
3. Create the autoscaling group with AWS AMI and user data to install and start the Nginx server
Create volumes for the instances in ASG to store application & log data can you 
4. Create S3 bucket to store the webcontent

File included are:
a)  main.tf: main.tf file contains the terraform script to create necessary resources.
b)  variables.tf: for declaring variables being used in the main script
c)  terraform.tfvars: for defining/overriding the varibles
d)  User data script for launch config which installs & starts nginx server and creates mount points and copies the index.html from my local directory to s3 bucket.
