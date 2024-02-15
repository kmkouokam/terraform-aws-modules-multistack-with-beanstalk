This module version (1.0.0) has no root configuration. A module with no root configuration cannot be used directly.

Use the submodules dropdown above to view the 5 submodules defined within this module.


Features
------------------------------------------------------------------------------------------------------------------------
1- Create mutlistack application with MySQL, Elasticached, and ActiveMQ in the backend, and Beanstalk in the frontend.

2- Iam role and policies for Beanstalk



Usage
------------------------------------------------------------------------------------------------------------------------
1- create a Directory called network. If you change the directory name, remember to adjust the path in the terraform block.

This module creates a network for the whole infrastructure. It has a public subnet in 2 AZ where the bastion and beanstalk instances will be deployed. A private subnet for backend resources

2- Set up the bastion host to log in to your RDS: create a Directory called bastion. If you change the directory name, remember to adjust the path in the terraform block.    

Add SSH rule to a bastion security group from MyIP
Login to bastion
clone the source code

Important: The network should have been created already because you need to refer to this data.terraform_remote_state.vpc in your bastion main.tf code file

Without the terraform local backend block, terraform will tell you that the state file cannot be read.



3-Set up your backend: create a directory called db, If you change the directory name, remember to adjust the path in the terraform block.

 

It will generate passwords for MySQL and ActiveMQ and store them in the parameter store in the system manager.

Will create MySQL, Elaticached, and ActiveMQ instances and a backend security group.

You must set up the data.terraform_remote_state.bastion, data.terraform_remote_state.vpc, and the terraform local backend for terraform apply to work without error messages.

The bastion module should be created before the db module.

Without this block: the terraform local backend, an error message will be generated saying the state file could not be read.


Login into your bastion host to initialize the DB with the schema  using the below command

clone the source code

commands: MySQL -h "DB endpoint" -u "DB username" -p"DB password" "DBaccountName" < db_backup.sql

Edit your application.properties file with the rds, elasticated, and activeMq endpoints as well as the password and username of your rds and activeMq.

built your artifact 



4- create a directory called iam_role, If you change the directory name, remember to adjust the path in the terraform block.


This module creates IAM role service and policies for Elastic Beanstalk and an instance profile.

The instance profile is used to pass an IAM role to an EC2 instance

5- set up the frontend: create a directory called frontend, If you change the directory name, remember to adjust the path in terraform block.

 The VPC, the Beanstalk service role, and the ec2 role should have been created already because you must refer them here using the data block.

 Without the terraform local backend block, terraform will tell you that the state file cannot be read.

The source bundle is needed for your elastic Beanstalk app. Found here
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html.

My domain name server was purchased in Goddaddy. I added a CNAME record and the value provided was my Beanstalk domain name. After provisioning your environment, edit the configuration to add listener https and provide your domain name certificate.

The path of the source bundle will be provided in your "aws_s3_object" resource in the source argument.

Important: The s3 bucket provisions in this code will store only the source bundle zip file. Beanstalk will create automatically its own bucket where the artifact and other resources will be stored. 


Author
--------------------------------------------------------------------------------------------------------------------------
Module is maintained by Ernestine D Motouom


