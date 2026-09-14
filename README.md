# Terraform
My Terraform progress
WordPress & Cloud-init assignment below

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


**EC2 Deployment with Cloud Init**

What I Built
I deployed an EC2 web server on AWS using Terraform and cloud init. Instead of embedding a bash script inside my Terraform file I created a separate YAML file that configures the server automatically on first boot. Cloud init installs Apache web server starts the service and creates a custom landing page with zero manual steps required.

Terraform Plan Output
<img width="1920" height="1028" alt="Assignment 2 1" src="https://github.com/user-attachments/assets/2ae0b003-b20f-4b16-be77-f5a58323f35e" />

Terraform Apply Success
<img width="1920" height="1027" alt="Assignment 2 2" src="https://github.com/user-attachments/assets/a02d1324-d88e-4ed0-98dd-884076644787" />

Cloud Init Webpage
<img width="1920" height="1020" alt="Assignment 2 0" src="https://github.com/user-attachments/assets/ec47186b-d5a1-46cc-95ce-55af1fc3b539" />


AWS Console EC2 Instance
<img width="1907" height="976" alt="Assignment 2 3" src="https://github.com/user-attachments/assets/f8a7281b-e5c0-4db9-beb8-e00cb72f69b4" />


Key Features
Infrastructure as Code every resource defined in Terraform files
External cloud init YAML file for cleaner configuration
Apache web server installed and started automatically
Custom welcome page created on boot
Security group allows HTTP from anywhere and SSH from my IP
Outputs public IP and website URL on deployment
Technologies Used
Terraform AWS EC2 VPC Apache Cloud Init YAML

**How to Deploy**
1 terraform init
2 terraform plan
3 terraform apply

Cloud Init File
Create a file named cloud-init.yaml with this content
#cloud-config
package update true
package upgrade true
packages
httpd
runcmd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from Cloud Init</h1>" > /var/www/html/index.html
Terraform File
Create a file named main.tf with this content
terraform
required providers
aws
source hashicorp/aws
version ~> 6.0
provider "aws"
region eu-west-2
resource "aws_vpc" "assignment2_vpc"
cidr_block 10.0.0.0/16
tags
Name Assignment2-VPC
resource "aws_subnet" "public"
vpc_id aws_vpc.assignment2_vpc.id
cidr_block 10.0.1.0/24
availability_zone eu-west-2a
map_public_ip_on_launch true
tags
Name Assignment2-Public
resource "aws_internet_gateway" "igw"
vpc_id aws_vpc.assignment2_vpc.id
tags
Name Assignment2-IGW
resource "aws_route_table" "public"
vpc_id aws_vpc.assignment2_vpc.id
route
cidr_block 0.0.0.0/0
gateway_id aws_internet_gateway.igw.id
tags
Name Assignment2-Public-RT
resource "aws_route_table_association" "public"
subnet_id aws_subnet.public.id
route_table_id aws_route_table.public.id
resource "aws_security_group" "web_sg"
name assignment2-sg
description Allow HTTP and SSH
vpc_id aws_vpc.assignment2_vpc.id
ingress
from_port 80
to_port 80
protocol tcp
cidr_blocks 0.0.0.0/0
ingress
from_port 22
to_port 22
protocol tcp
cidr_blocks 92.31.151.55/32
egress
from_port 0
to_port 0
protocol -1
cidr_blocks 0.0.0.0/0
tags
Name assignment2-sg
resource "aws_instance" "web_server"
ami ami-0f9629c639a701fa7
instance_type t3.micro
subnet_id aws_subnet.public.id
vpc_security_group_ids [aws_security_group.web_sg.id]
associate_public_ip_address true
user_data file("cloud-init.yaml")
tags
Name Cloud-Init-Server
output "public_ip"
value aws_instance.web_server.public_ip
output "website_url"
value http://${aws_instance.web_server.public_ip}

**What I Learnt**
I learnt the difference between embedding scripts in Terraform and using an external cloud init file. Cloud init uses standard YAML format which is cleaner more readable and widely supported across cloud platforms. I understood how Terraform passes configuration files to EC2 instances at launch time and how cloud init runs automatically before the server becomes available. This approach keeps infrastructure code separate from configuration making it easier to update and reuse.

Challenges and Solutions
Wrong AWS region in console — resolved by switching to Europe London eu-west-2
Cloud init taking time to run — resolved by waiting 3 to 5 minutes before testing the website
Incorrect file path — resolved by keeping main.tf and cloud-init.yaml in the same folder
Security group SSH access too open — resolved by restricting SSH to my IP address only
Clean Up
Run terraform destroy when finished to remove all resources and avoid charges
