# Terraform
My Terraform progress

**WordPress Deployment with Terraform**

What I Built
I deployed a complete WordPress web application on AWS using only Terraform code. The configuration includes a custom VPC public subnet internet gateway routing tables security groups and an EC2 instance. User data scripts automatically install Apache PHP MariaDB and WordPress during launch zero manual database setup required.

Terraform Plan Output
<img width="1920" height="1028" alt="Assignment 3" src="https://github.com/user-attachments/assets/bef0b9f3-6c17-49ed-b241-48e83a0cc7f4" />

Terraform Apply Success
<img width="1920" height="1031" alt="Assignment 2" src="https://github.com/user-attachments/assets/238031fd-0c5c-49c1-ace2-ad5e058b5828" />

WordPress Setup Screen
<img width="1920" height="979" alt="Assignment 1" src="https://github.com/user-attachments/assets/dd8b667c-a9c2-4f0d-a5bd-fe0e282ecec6" />

AWS Console EC2 Dashboard
<img width="1920" height="981" alt="Assignment 4" src="https://github.com/user-attachments/assets/0feb1afe-8caf-48cd-be73-b33cc8a1f1cb" />

Key Features
Infrastructure as Code every resource defined in Terraform files
Auto generated VPC and networking
Security groups allowing web traffic from anywhere and restricted SSH access from my IP
Fully automated WordPress and database installation on first boot
Outputs public IP and access URL on deployment
Technologies Used
Terraform AWS EC2 VPC Apache PHP MariaDB WordPress
How to Deploy
1 terraform init
2 terraform plan
3 terraform apply

**What I Learnt**
I learnt how to define complete cloud infrastructure in code rather than clicking through the AWS console. I understood how Terraform manages state comparing your written configuration against what actually exists and how to provision networking and security alongside compute resources. Troubleshooting credential errors instance types and subnet configuration taught me how all the AWS pieces connect together.
Challenges and Solutions
AWS credentials were initially invalid resolved by running aws configure with correct access keys
Default VPC had no subnets resolved by defining a complete VPC and subnet in Terraform
Instance type was not Free Tier eligible changed from t2.micro to t3.micro for eu west 2 region
Database connection errors resolved by adding MariaDB installation and automatic wp config php generation to the user data script
