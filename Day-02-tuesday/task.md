## Task 1 :  Restrict EC2 Actions to Specific Regions (Conditional Policy)

Objective :

Allow EC2 actions **only in two specific regions** using IAM policy conditions.

Allowed Regions:

* ap-south-1 (Mumbai)
* us-east-1 (N. Virginia)

Policy Used:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": [
            "ap-south-1",
            "us-east-1"
          ]
        }
      }
    }
  ]
}
```

Result:

* EC2 resources can be created **only in ap-south-1 and us-east-1**
* Attempts in other regions are denied automatically.

---

## Task 2 : IAM Access with Permission Boundary

Objective:

Create a user policy that allows IAM access but enforce restrictions using a **Permission Boundary**.

User Policy Result (Allowed Actions Observed):

```
"iam:ListPolicies",
"iam:GetRole",
"iam:PutUserPermissionsBoundary",
"iam:AttachUserPolicy",
"iam:AttachRolePolicy",
"iam:CreateUser",
"iam:CreateLoginProfile",
"iam:DetachRolePolicy",
"iam:ListUsers",
"iam:GetUser",
"iam:DetachUserPolicy",
"iam:DeleteUser"
```

Permission Boundary Applied:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyDeleteUser",
      "Effect": "Deny",
      "Action": "iam:DeleteUser",
      "Resource": "*"
    },
    {
      "Sid": "AllowMinimalIAMActions",
      "Effect": "Allow",
      "Action": [
        "iam:CreateUser",
        "iam:AttachUserPolicy",
        "iam:ListUsers",
        "iam:ListGroups",
        "iam:ListPolicies"
      ],
      "Resource": "*"
    }
  ]
}
```

Final Result:

* User can perform several IAM actions.
* **DeleteUser action is denied because Permission Boundary overrides the allow permission.**

Key Concept Demonstrated:

```
Permission Boundary = Maximum Allowed Permissions
```

Even if the IAM policy allows an action, the permission boundary can **block it**.

---

## Task 3 : Cross Account Access using STS AssumeRole

Objective:

Access resources in another AWS account using **STS AssumeRole**.

Architecture:

```
Destination Account (User)
        │
        │ sts:AssumeRole
        ▼
Source Account (IAM Role)
        │
        ▼
Access permitted resources
```

---

### Source Account Role Policy

Role: `source-role`

Policy attached to role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "iam:ListUsers",
      "Resource": "*"
    }
  ]
}
```

This role allows listing IAM users in the source account.

---

### Destination Account User Policy

Policy attached to the user who wants cross-account access:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::887845961215:role/source-role"
    }
  ]
}
```

Result:

* User from destination account assumes the role in the source account.
* Temporary STS credentials are generated.
* User can perform actions defined in the **source-role policy**.

---