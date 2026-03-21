Topic: Designing, Developing, and Deploying a Containerized Application using AWS ECS, ECR, EKS, and RDS

Objective

In this lab we designed, developed, deployed, and operated a containerized application on AWS using modern cloud-native services.

The application architecture includes:

Docker for containerization
Amazon ECR for container image storage
Amazon ECS (Fargate) for container orchestration
Amazon EKS (Kubernetes) for container orchestration
Amazon RDS as the managed relational database
The goal is to deploy the same containerized application using both ECS and EKS while integrating it with an AWS-managed database.

Task 1 : Application Setup and ECR
![alt text](<Screenshot 2026-03-17 121331-1.png>)

![alt text](<Screenshot (189).png>)

task 2 : Deploying using ECS
![alt text](<Screenshot (191).png>)



Task 3 : Deploy same application in EKS

Step-1: Create EC2 Instance for Cluster Management
Install required tools:

sudo yum install aws-cli -y
Install kubectl.

curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
Install eksctl.

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
Step-2: Create EKS Cluster
eksctl create cluster --name <cluster-name> --region <region-name> --node-type <instance-type> --nodes <desired-node-count>
Cluster creation may take 10–15 minutes.

Step-3: Create Kubernetes Deployment
Create file:

deployment.yaml
Add deployment configuration.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: crm-app
  labels:
    app: crm-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: crm-app
  template:
    metadata:
      labels:
        app: crm-app
    spec:
      containers:
        - name: crm-app
          image: test:1.0
          ports:
            - containerPort: 3000
          env:
            - name: PORT
              value: "3000"
            - name: DB_HOST
              value: "database-1.cydccqm041wu.us-east-1.rds.amazonaws.com"
            - name: DB_PORT
              value: "3306"
            - name: DB_USER
              value: "admin"
            - name: DB_PASSWORD
              value: "admin123"
            - name: DB_NAME
              value: "crm_db"
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
          livenessProbe:
            httpGet:
              path: /api/health
              port: 3000
            initialDelaySeconds: 15
            periodSeconds: 30
          readinessProbe:
            httpGet:
              path: /api/health
              port: 3000
            initialDelaySeconds: 10
            periodSeconds: 10


Step-4: Create Kubernetes Service
Create file:

service.yaml
Add service configuration.

apiVersion: v1
kind: Service
metadata:
  name: crm-app-service
  labels:
    app: crm-app
spec:
  selector:
    app: crm-app
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000

      
Step-5: Deploy Application
Apply deployment.

kubectl apply -f deployment.yaml
Apply service.

kubectl apply -f service.yaml


Step-6: Verify Deployment
Check pods.

kubectl get pods
Check services.

kubectl get svc
Step-7: Access Application
Once the service is created, Kubernetes provides a Load Balancer endpoint.

Example:

http://<load-balancer-endpoint>


![alt text](<Screenshot (193).png>)