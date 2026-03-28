# AWS Day 2

Topics Covered:

1. AWS CloudWatch Overview
2. CloudWatch Dashboards
3. CloudWatch Services
4. CloudWatch Alarms
5. CloudWatch Metrics
6. IAM Overview
7. STS, Permission Boundaries, and Conditional Policies

---

# 1. AWS CloudWatch Overview

**Amazon CloudWatch** is a monitoring and observability service in AWS used to monitor resources, applications, and services in real time.

It collects and tracks:

* Metrics
* Logs
* Events
* Alarms

CloudWatch helps administrators and DevOps engineers:

* Monitor infrastructure health
* Troubleshoot performance issues
* Automate actions based on alerts

Example monitored services:

* EC2 CPU utilization
* RDS connections
* Lambda execution duration
* S3 request metrics

---

# 2. CloudWatch Dashboard

A **CloudWatch Dashboard** is a customizable visual interface used to monitor AWS resources and applications in one place.

It displays metrics in visual form such as graphs and charts.

Example Dashboard Layout

```
+--------------------------------+
|        CloudWatch Dashboard    |
+--------------------------------+
| EC2 CPU Usage   | RDS Memory   |
| Graph           | Graph        |
+--------------------------------+
| Lambda Errors   | Network I/O  |
| Graph           | Graph        |
+--------------------------------+
```

### Steps to Create Dashboard

1. Open CloudWatch
2. Select **Dashboards**
3. Click **Create Dashboard**
4. Enter dashboard name
5. Add widgets

### Types of Dashboard Widgets

* Line Graph
* Stacked Area
* Number Display
* Text Widget
* Log Query Results

These widgets help visualize real-time AWS data.

---

# 3. CloudWatch Services

CloudWatch provides multiple monitoring services.

### CloudWatch Metrics

Numerical data representing AWS resource performance.

Example:

* CPU utilization
* Disk read/write
* Network traffic

---

### CloudWatch Logs

Collects and stores log files from AWS services.

Examples:

* EC2 application logs
* Lambda execution logs
* VPC Flow logs

---

### CloudWatch Alarms

Used to trigger alerts when metrics cross defined thresholds.

Example:

If CPU utilization > 80% → send notification.

---

### CloudWatch Events / EventBridge

Allows automation based on events.

Example:

* EC2 instance state change
* Scheduled automation tasks

---

### CloudWatch Insights

Powerful query tool for analyzing logs.

Used to quickly search and analyze large log data.

---

# 4. CloudWatch Alarms

A **CloudWatch Alarm** monitors a metric and performs actions when thresholds are crossed.

Example alarm configuration:

Metric: EC2 CPU Utilization

Threshold:

```
CPU > 80%
Duration: 5 minutes
```

If the condition becomes true, the alarm enters **ALARM state**.

### Alarm States

```
OK
ALARM
INSUFFICIENT_DATA
```

### Alarm Actions

When triggered, alarms can:

* Send SNS notifications
* Trigger Auto Scaling
* Stop or terminate EC2
* Execute Lambda functions

### Alarm Architecture

```
Metric
  ↓
Threshold Evaluation
  ↓
Alarm Trigger
  ↓
SNS Notification / Automation
```

---

# 5. CloudWatch Metrics

**Metrics** are time-ordered data points representing system performance.

AWS services automatically send metrics to CloudWatch.

Example metrics:

| Service | Metric Example      |
| ------- | ------------------- |
| EC2     | CPUUtilization      |
| RDS     | DatabaseConnections |
| Lambda  | Duration            |
| S3      | BucketSizeBytes     |

### Types of Metrics

1. **Default Metrics** – Automatically created by AWS services
2. **Custom Metrics** – Created by users or applications

Example Custom Metric Use Case

Application sends request count metric to CloudWatch.

### Metric Data Structure

```
Namespace
   ↓
Metric Name
   ↓
Dimensions
   ↓
Timestamp + Value
```

---

# 6. IAM (Identity and Access Management)

**IAM** is AWS service used to securely manage access to AWS resources.

IAM defines:

* Who can access AWS
* What actions they can perform

---

### IAM Resources

IAM manages the following identities:

1. Users
2. Groups
3. Roles
4. Policies

Example IAM Structure

```
AWS Account
   │
   ├── IAM Users
   ├── IAM Groups
   ├── IAM Roles
   └── IAM Policies
```

---

### Types of IAM Policies

Policies define permissions in JSON format.

Main policy types:

1. AWS Managed Policies
2. Customer Managed Policies
3. Inline Policies

#### AWS Managed Policy

Predefined policies created by AWS.

Example:

AdministratorAccess

---

#### Customer Managed Policy

Policies created and managed by users.

Reusable across multiple IAM entities.

---

#### Inline Policy

Policy attached directly to one user, group, or role.

Not reusable.

---

# 7. Self Study and Practice Tasks

This section was explored as **self‑study and hands‑on tasks** to better understand advanced IAM security controls.

Topics included:

* AWS STS (Security Token Service)
* IAM Permission Boundaries
* IAM Conditional Policies

---

## AWS STS (Security Token Service)

AWS **STS** provides **temporary security credentials** that allow users to access AWS resources for a limited time.

Temporary credentials include:

* Access Key
* Secret Key
* Session Token

Common use cases:

* Cross‑account access
* Temporary administrative access
* Federated user login

Example Flow

```
User
  ↓
Assume Role using STS
  ↓
Temporary Credentials Generated
  ↓
Access AWS Resources for limited time
```

---

## IAM Permission Boundaries

A **Permission Boundary** defines the **maximum permissions** that an IAM user or role can have.

Even if an IAM policy allows certain permissions, the permission boundary restricts the final effective permissions.

Example

```
IAM Policy → Allow EC2 + S3
Permission Boundary → Allow only S3

Final Result → Only S3 is allowed
```

This is useful in large organizations where administrators want to **delegate IAM creation but still enforce limits**.

---

## IAM Conditional Policies

IAM policies can include **conditions** to control when a permission should be allowed.

Common conditions include:

* Source IP address
* Time of request
* MFA authentication
* Requested AWS region

Example Logic

```
Allow S3 Access
ONLY IF
MFA is enabled
```

Example Use Case

Allow access only from corporate office IP range.

---