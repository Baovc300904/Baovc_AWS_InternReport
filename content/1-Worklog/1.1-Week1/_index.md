---
title: "Week 1 Worklog"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 1 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Get acquainted with FCJ members <br> - Read and take note of internship unit rules and regulations <br> - Set up development environment (Git, VS Code)      | 08/09/2025 | 08/09/2025      | FCJ Internal Documentation                |
| 3   | - Learn about AWS and its types of services <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC) <br>&emsp; + Database (RDS, DynamoDB) | 09/09/2025 | 09/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account <br> - Learn about AWS Console & AWS CLI <br> - **Practice:** <br>&emsp; + Create AWS account <br>&emsp; + Install & configure AWS CLI <br>&emsp; + Test basic CLI commands | 10/09/2025 | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic EC2: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + Security Groups <br> - SSH connection methods to EC2 <br> - Learn about Elastic IP   | 11/09/2025 | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Practice:** <br>&emsp; + Launch an EC2 instance <br>&emsp; + Connect via SSH <br>&emsp; + Attach an EBS volume <br>&emsp; + Test basic operations | 12/09/2025 | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 1 Achievements:

* **Understood AWS Cloud Computing basics:**
  * Learned what AWS is and its role as a cloud service provider
  * Explored core service categories:
    * Compute (EC2, Lambda)
    * Storage (S3, EBS)
    * Networking (VPC)
    * Database (RDS, DynamoDB)
    * Security (IAM, Security Groups)

* **Set up AWS account and environment:**
  * Created AWS Free Tier account successfully
  * Configured basic security settings
  * Set up billing alerts to monitor costs

* **Got familiar with AWS Management Console:**
  * Learned to navigate the AWS Console interface
  * Found and accessed different AWS services
  * Understood basic service dashboards

* **Installed and configured AWS CLI:**
  * Installed AWS CLI version 2 on local machine
  * Created IAM user credentials
  * Configured AWS CLI with:
    * Access Key ID
    * Secret Access Key
    * Default Region (ap-southeast-1)
  * Verified configuration with basic commands

* **Practiced with AWS CLI commands:**
  * `aws sts get-caller-identity` - Check account info
  * `aws ec2 describe-regions` - List AWS regions
  * `aws ec2 describe-instances` - View EC2 instances
  * `aws s3 ls` - List S3 buckets
  * `aws ec2 describe-key-pairs` - Manage SSH keys

* **Learned EC2 basics:**
  * Understood different EC2 instance types (t2, t3, m5)
  * Learned about AMI (Amazon Machine Images)
  * Studied EBS volumes and their types
  * Understood Security Groups for firewall rules

* **Completed EC2 hands-on practice:**
  * Launched EC2 instance (t2.micro)
  * Configured Security Group for SSH access
  * Connected to EC2 via SSH
  * Created and attached EBS volume
  * Tested basic Linux commands on EC2

* **Gained foundational AWS skills:**
  * Can use both Console and CLI to manage resources
  * Understand basic AWS concepts and terminology
  * Ready to learn more advanced topics in Week 2