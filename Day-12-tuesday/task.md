Deploy EC2 Instance with Complete Network Infrastructure, UserData Web Server & SSH Password Configuration using AWS CloudFormation

Deployed a complete AWS infrastructure using a CloudFormation Template (CFT) that provisions a VPC with all networking components, launches an EC2 instance, installs Nginx as a web server, and configures SSH to allow password-based login — all automated via UserData.

Infrastructure Created via CFT
Resource	Name	Details
VPC	MyVPC	CIDR: 10.0.0.0/16, DNS enabled
Internet Gateway	MyIGW	Attached to MyVPC
Public Subnet 1	PublicSubnet1	10.0.1.0/24 — AZ-0
Public Subnet 2	PublicSubnet2	10.0.2.0/24 — AZ-1
Route Table	PublicRT	0.0.0.0/0 → IGW
RT Association 1	SubnetRouteTableAssociation1	PublicSubnet1 ↔ PublicRT
RT Association 2	SubnetRouteTableAssociation2	PublicSubnet2 ↔ PublicRT
Security Group	MySecurityGroup	Port 22 (SSH), Port 80 (HTTP)
EC2 Instance	MyEC2Instance	t3.micro, Amazon Linux 2023

