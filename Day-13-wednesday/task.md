Task-1
Create a 3-tier architecture

VPC
Public EC2 (Web Layer)
Private RDS (Database Layer)
Secure DB access using Security Groups

Use Parameters for sensitive data (NoEcho)

Delpoying the stack :

aws cloudformation deploy \
  --template-file three-tier-architecture-clean.yaml \
  --stack-name my-three-tier-stack \
  --parameter-overrides \
      KeyPairName=first-key \
      DBUsername=adminuser \
      DBPassword=Secur3Pass! \
  --capabilities CAPABILITY_IAM


Task-2

Create Linux / Windows / Both EC2 instances dynamically
Use Conditions, Mappings, and Metadata
Configure user-based login with username/password
Deploy web servers automatically using User Data

Deploying the stack :

for linux :

aws cloudformation deploy \
  --template-file task2-dynamic-ec2.yaml \
  --stack-name task2-linux \
  --parameter-overrides \
      InstanceOS=Linux \
      Environment=Dev \
      VpcId=vpc-0055cf2c78355a7b7 \
      SubnetId=subnet-008faae0950bc7f6c \
      KeyPairName=first-key \
      WebServerUsername=webadmin \
      WebServerPassword=Admin@1234 \
  --capabilities CAPABILITY_IAM

  
for windows :

aws cloudformation deploy \
  --template-file task2-dynamic-ec2.yaml \
  --stack-name task2-windows \
  --parameter-overrides \
      InstanceOS=Windows \
      Environment=Dev \
      VpcId=vpc-0055cf2c78355a7b7 \
      SubnetId=subnet-008faae0950bc7f6c \
      KeyPairName=first-key \
      WebServerUsername=webadmin \
      WebServerPassword=Admin@1234 \
  --capabilities CAPABILITY_IAM