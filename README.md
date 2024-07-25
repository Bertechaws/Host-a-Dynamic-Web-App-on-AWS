![image](https://github.com/user-attachments/assets/b1438d60-4975-4e92-8607-28d97f39de8a)


 Hosting a Dynamic Website on AWS

### Overview
This project involves hosting a dynamic website on AWS using a variety of services and configurations to ensure scalability, reliability, and security.

### Project Components and Configurations
1. **Virtual Private Cloud (VPC)**:
   - Configured with public and private subnets spanning two availability zones for high availability.

2. **Internet Gateway**:
   - Deployed to enable connectivity between VPC instances and the internet.

3. **Security Groups**:
   - Implemented as network firewall mechanisms to control inbound and outbound traffic.

4. **Availability Zones**:
   - Utilized two availability zones to enhance system reliability and fault tolerance.

5. **Subnets**:
   - Public subnets housed infrastructure components like NAT Gateway and Application Load Balancer.
   - Private subnets hosted web servers (EC2 instances) for enhanced security.

6. **EC2 Instance Connect Endpoint**:
   - Implemented for secure connections to assets within both public and private subnets.

7. **NAT Gateway**:
   - Enabled instances in private subnets (Application and Data) to access the internet securely.

8. **Website Hosting**:
   - Hosted on EC2 instances within the private subnets.

9. **Application Load Balancer (ALB)**:
   - Used with a target group to evenly distribute web traffic across an Auto Scaling Group of EC2 instances.

10. **Auto Scaling Group**:
    - Managed EC2 instances automatically to ensure website availability, scalability, fault tolerance, and elasticity.

11. **Certificate Manager**:
    - Secured application communications with SSL/TLS certificates.

12. **Simple Notification Service (SNS)**:
    - Configured to alert about activities within the Auto Scaling Group.

13. **Domain Registration and DNS**:
    - Registered domain name and set up DNS record using Route 53.

14. **Code Management**:
    - Used S3 to store application codes for deployment.

### Data Migration Using Flyway Script
The provided Bash script demonstrates migrating data from an S3 bucket to an RDS database using Flyway:

```bash
#!/bin/bash

S3_URI=s3://berny-project-web-files/V1__shopwise.sql
RDS_ENDPOINT=rdsdb.c4fwppnob0ct.us-east-1.rds.amazonaws.com
RDS_DB_NAME=devdatabase
RDS_DB_USERNAME=berny
RDS_DB_PASSWORD=berny123

# Update all packages
sudo yum update -y

# Download and extract Flyway & Create a symbolic link to make Flyway accessible globally
sudo wget -qO- https://download.red-gate.com/maven/release/com/redgate/flyway/flyway-commandline/10.16.0/flyway-commandline-10.16.0-linux-x64.tar.gz | tar -xvz && sudo ln -s `pwd`/flyway-10.16.0/flyway /usr/local/bin 

# Create the SQL directory for migrations
sudo mkdir sql

# Download the migration SQL script from AWS S3
sudo aws s3 cp "$S3_URI" sql/

# Run Flyway migration
flyway -url=jdbc:mysql://"$RDS_ENDPOINT":3306/"$RDS_DB_NAME" \
  -user="$RDS_DB_USERNAME" \
  -password="$RDS_DB_PASSWORD" \
  -locations=filesystem:sql \
  migrate
```

### Conclusion
This project demonstrates comprehensive utilization of AWS services and best practices to host and manage a dynamic website efficiently. The combination of VPC setup, load balancing, auto scaling, secure data migration, and continuous integration enhances the website's performance, reliability, and scalability in a cloud environment.
