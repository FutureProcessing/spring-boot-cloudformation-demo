# Spring Boot CloudFormation demo
This project demonstrates how to deploy Spring Boot app (generated on http://start.spring.io/) in AWS using:
 
* Elastic Beanstalk
* VPC
* CloudFormation

### Running on AWS
 
1. Run `mvn clean package` and upload generated war to S3 bucket (for example using aws-cli)
2. Go to CloudFormation and create a new stack using `resources/aws/create_environment.template`.
3. Provide stack input parameters: keypair names (must be created manually before) and location of war file (bucket and key).
4. Wait for stack to be ready.
5. Check `ApplicationUrl` stack output, this is an URL where the app is deployed.
 
### About template
 
Running mentioned template creates:
 
* VPC in 3 availability zones, one public and private subnet per AZ.
* HA NAT that allows outbound network traffic from private subnets (based on AWS [article](https://aws.amazon.com/articles/6079781443936876)).
* Bastion host. It servers as a proxy for ssh connections (instances in private subnets cannot be reached directly from outside of the VPC).
* Demo bucket.
* Backend instance profile allowing backend instances all operations on demo bucket. 
* Backend beanstalk environment inside VPC with initial version (.war file passed as input parameter).

License
=======

    Copyright 2015 Future Processing Sp. z o.o.

    Licensed under The MIT License (MIT), see LICENSE.txt for details.