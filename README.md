# CloudFormation

My CloudFormation Templates

# Objective
Use CM tools such as Puppet, Ansible, or Chef to automate the installation of basic Drupal or WordPress. Setup a sample site. Automate the solution using Cloudformation template.

# Deliverable

A cloudformation template that accepts user inputs as parameters where applicable (for example, Admin password). This template should setup VPC, create subnets, launch a CM instance, pull the necessary code (modules, classes, recipes etc) from a GIT repo (or S3), and configure the web instance for basic Drupal or Wordpress setup.

# Description and Approach

As part of the Deliverable I have created two Templates, Below are the details :

ik_Rean_SingleInstance.yaml (Works successfully)- This template will :
  1. Launch a VPC with public and private subnets with route table associations for each subnet.
  2. Nat gateway in public subnet. And the private subnet will have a route in the route table for traffic outside of the VPC/public internet going through the NAT Gateway
  4. 1 web server instance in public subnet which will have Chef and WordPress application installed. The mysql database is installed locally in the same server.
  5. Outputs section will display the Website URL which can be used to accessed the wordpress site successfully.

ik_Rean_SeparateDBInstance.yaml (Partially successful): This template will :
  1. Launch a VPC with public and private subnets with route table associations for each subnet.
  2. Nat gateway in public subnet. And the private subnet will have a route in the route table for traffic outside of the VPC/public internet going through the NAT Gateway.
  3. 1 webserver instance in public subnet and 1 DB server instance in private subnet.
  4. The web server instance will have the Chef and Wordpress website configured and the DB server will have mysql installation as a wordpress database.
  5. Outputs section will display the website URL.
Note: The CF stack will create successfully. The wordpress website is not launching as I am unable to figure out the recipes which can automate the configuration of the mysql DB server for Wordpress website. I was able to achieve this using RDS by modifying amazon sample template, but in the candidate account the RDS access IAM policy is not assigned.

# Usage

The templates can be executed by :

1. Using the Cloud formation service my providing the below S3 URL or by uploading the template file
   - https://s3-us-west-2.amazonaws.com/ik-rean-bucket/ik_Rean_SingleInstance.yaml

2. using below AWS CLI command :

aws cloudformation create-stack --stack-name ik-Rean-Stack --template-url https://s3-us-west-2.amazonaws.com/ik-rean-bucket/ik_Rean_SingleInstance.yaml --parameters  ParameterKey=DBName,ParameterValue=wordpressdb ParameterKey=DBPassword,ParameterValue=admin123 ParameterKey=DBRootPassword,ParameterValue=admin123 ParameterKey=DBUser,ParameterValue=admin ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=KeyName,ParameterValue=ik-Rean_Oregon ParameterKey=SSHLocation,ParameterValue=10.0.0.240/32


# Conditions

The templates are only executable in us-west-2 or us-west-1 regions as I have only added 2 regions for the AMI's in mappings.
This can be configured for more regions if required under the mappings section.
