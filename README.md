# databasemigration-etl-blog
Resources for AWS Database Migration Service and AWS Glue Blog by Sona Rajamani.

This repo includes two CloudFormation templates that can be deployed in US-East-2 (Ohio) or US-West-2 (Oregon) region. It also includes a PySpark script, which performs the ETL action in AWS Glue job. 

Details about the files in this repo:

## 1. migrationresource.template
This template deploys a stack consisting of:
	1.	a) VPC
	1.	b) IGW - Internet Gateway
	1.	c) IAM Role for migration
	1.	d) 2 Public Subnets in two different AZ within a region - [PublicSubnet1, PublicSubnet2] 
	1.	e) Route tables
	1.	f) VPC S3 Endpoint	   
	1.	g) Security Group for EC2 instance
	1.	h) Elatic IP for EC2 instance
	1.	i) EC2 instance from an AMI preconfigured with Oracle XE and HRDATA database.
	1.	j) IAM instance profile
	1.	k) S3 Bucket 
	1.	l) DB Subnet Group
	1.	m) RDS Aurora MySQL Cluster
	1.	n) RDS Aurora MySQL DB Instance
	1.	o) Replication Subnet Group
	1.	p) Replication Instance

Design:
![GitHub Logo](/images/migrationresourcesTemplate.png)
Format: ![Alt Text](https://s3.us-east-2.amazonaws.com/blog-scripts-glueetl/cfscripts/img/migrationresourcesTemplate.png)


## 1. glueblog.template
This template deploys a stack consisting of resources in AWA Glue.  The resources are created based on the output of migrationresource stack.  
	1.	a) AWS Glue RDS Amazon Aurora MySQL JDBC connection
	1.	b) AWS Glue database named hrdb
	1.	c) AWS Glue crawler 
	1.	d) AWS Glue ETL Job - The job uses PySpark [blogetl.py] script stored in a region specific s3 bucket for ETL in the job.

Design:
![GitHub Logo](/images/glueblogTemplate.png)
Format: ![Alt Text](https://s3.us-east-2.amazonaws.com/blog-scripts-glueetl/cfscripts/img/glueblogTemplate.png)


## 1. blogetl.py
Script that does an inner join of EMPLOYEES and DEPARTMENTS tables in HRDATA database and writes the results of the join a new table called EMPLOYEES_DEPARTMENTS in RDS Amazon Aurora MySQL database.	   
