# Disaster Recovery Architecture with Cross-Region Replication and Automated Failover

## Project Overview

This project demonstrates a production-grade Disaster Recovery (DR) architecture using Amazon Web Services (AWS). The solution ensures business continuity by replicating application data from the Primary Region (US East - N. Virginia) to the Disaster Recovery Region (US East - Ohio). In the event of a primary region failure, the application can be recovered with minimal downtime and minimal data loss.

---

# Project Objective

- Deploy a highly available web application.
- Configure Amazon RDS MySQL database.
- Configure Amazon S3 bucket for object storage.
- Enable S3 Cross-Region Replication (CRR).
- Configure Amazon RDS Cross-Region Read Replica.
- Deploy Disaster Recovery infrastructure.
- Simulate disaster and perform failover.
- Validate Recovery Time Objective (RTO) and Recovery Point Objective (RPO).

---

# AWS Services Used

- Amazon EC2
- Amazon VPC
- Amazon RDS MySQL
- Amazon S3
- Amazon S3 Cross-Region Replication
- AWS IAM
- Amazon Route 53 (Optional)
- Security Groups

---

# Primary Region

**Region**

US East (N. Virginia)

```
us-east-1
```

Resources

- VPC
- Internet Gateway
- Public Subnet
- Route Table
- EC2 Instance
- Security Group
- Amazon RDS MySQL
- Amazon S3 Bucket

---

# Disaster Recovery Region

**Region**

US East (Ohio)

```
us-east-2
```

Resources

- EC2 Instance
- Amazon RDS Read Replica
- S3 Destination Bucket

---

# Architecture Diagram

```
                           Users
                              |
                              |
                        Public IP / Route53
                              |
              -----------------------------------
              |                                 |
              |                                 |
      Primary Region                     Disaster Recovery Region
      us-east-1 (N. Virginia)            us-east-2 (Ohio)

      +-------------------+             +-------------------+
      |   EC2 Web Server  |             |   EC2 Web Server  |
      +---------+---------+             +---------+---------+
                |                                 |
                |                                 |
      +---------v---------+             +---------v---------+
      | Amazon RDS MySQL  |===========> | Amazon RDS Replica |
      |    Primary DB     | Replication |    Read Replica    |
      +-------------------+             +-------------------+

                |
                |
      +---------v---------+
      | Amazon S3 Bucket  |
      +-------------------+
                |
                |
     Cross Region Replication
                |
                |
      +---------v---------+
      | S3 Bucket (Ohio)  |
      +-------------------+
```

---

# Project Implementation

## Step 1

Created VPC in Primary Region.

- Internet Gateway
- Public Subnet
- Route Table
- Security Group

---

## Step 2

Launched Ubuntu EC2 Instance.

Installed Apache, PHP and MySQL Extension.

```bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php php-mysql -y
```

---

## Step 3

Removed Apache Default Page.

```bash
sudo rm /var/www/html/index.html
```

---

## Step 4

Created PHP Application.

```bash
cd /var/www/html
sudo nano index.php
```

---

## Step 5

Restarted Apache.

```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```

---

## Step 6

Created Amazon RDS MySQL Database.

Database Name

```
healthcaredb
```

Created Table

```
patients
```

Inserted Sample Records.

---

## Step 7

Connected EC2 to Amazon RDS.

```bash
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
```

---

## Step 8

Created Amazon S3 Bucket.

Bucket Name

```
dr-healthcare-storage-aditya-001
```

Enabled Versioning.

---

## Step 9

Configured Cross Region Replication.

Source Region

```
us-east-1
```

Destination Region

```
us-east-2
```

Uploaded

```
text.txt
```

Verified successful replication in Ohio bucket.

---

## Step 10

Created Amazon RDS Cross-Region Read Replica.

Source

```
N. Virginia
```

Destination

```
Ohio
```

Replication completed successfully.

---

## Step 11

Launched EC2 Instance in Ohio Region.

Installed Apache and PHP.

```bash
sudo apt update

sudo apt install apache2 php libapache2-mod-php php-mysql -y
```

Removed Apache Default Page.

```bash
sudo rm /var/www/html/index.html
```

Created PHP Application.

```bash
sudo nano /var/www/html/index.php
```

Restarted Apache.

```bash
sudo systemctl restart apache2
```

Checked Apache Status.

```bash
sudo systemctl status apache2
```

Verified Application.

```bash
curl http://localhost
```

Verified Database Connectivity.

```bash
nc -zv dr-mysql-db-replica.cjko2koac7lq.us-east-2.rds.amazonaws.com 3306
```

Connected to DR Database.

```bash
mysql --ssl-mode=DISABLED \
-h dr-mysql-db-replica.cjko2koac7lq.us-east-2.rds.amazonaws.com \
-P 3306 \
-u admin \
-p
```

Website successfully displayed patient records from the Disaster Recovery database.

---

# Disaster Simulation

Performed Disaster Recovery Testing.

- Stopped Primary EC2 Instance.
- Simulated Primary Region Failure.
- Activated Disaster Recovery Region.
- Accessed Ohio EC2.
- Verified application connectivity with Amazon RDS Read Replica.
- Confirmed successful recovery.

---

# Validation

Successfully Validated

- Primary Web Application
- Amazon RDS Connectivity
- Amazon S3 Bucket
- Cross Region Replication
- Amazon RDS Read Replica
- Disaster Recovery Web Application
- Disaster Simulation

---

# Recovery Time Objective (RTO)

Approximately

```
5–10 Minutes
```

---

# Recovery Point Objective (RPO)

Approximately

```
Less than 5 Minutes
```

---

# Disaster Recovery Strategy

| Strategy | Description |
|----------|-------------|
| Backup & Restore | Lowest Cost, Highest Recovery Time |
| Pilot Light | Minimal Infrastructure Running |
| Warm Standby | Scaled Down Environment Running |
| Multi-Site Active/Active | Highest Availability and Cost |

**Implemented Strategy**

```
Warm Standby with Cross-Region Replication
```

---

# Lessons Learned

- Designed highly available cloud infrastructure.
- Implemented Disaster Recovery architecture.
- Configured Cross Region Replication.
- Configured Amazon RDS Read Replica.
- Learned RTO and RPO concepts.
- Validated Disaster Recovery successfully.
- Improved AWS networking and infrastructure skills.

---

# Project Outcome

Successfully designed and implemented a Disaster Recovery Architecture using AWS.

Achievements

- Cross Region Data Replication
- Disaster Recovery Environment
- Amazon RDS Read Replica
- Amazon S3 Cross Region Replication
- Disaster Simulation
- Recovery Validation
- High Availability Architecture

---

# Author

**Name:** Aditya

**Role:** Cloud Reliability Engineer (Intern)

**Platform:** Amazon Web Services (AWS)

**Primary Region:** us-east-1 (N. Virginia)

**Disaster Recovery Region:** us-east-2 (Ohio)
