# AWS SCPs
Service control policies (SCPs) are meant to be used as coarse-grained guardrails, and they don’t directly grant access. The administrator must still attach identity-based or resource-based policies to IAM principals or resources in your accounts to actually grant permissions. The effective permissions are the logical intersection between what is allowed by the SCP and what is allowed by the IAM and resource-based policies. You can get more details about SCP effects on permissions here.

# aws-scp-examples
AWS SCPs Examples:

1. `Deny Account Region Enable and Disable Actions:`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "account:EnableRegion",
                "account:DisableRegion"
            ],
            "Resource": "*",
            "Condition": {
                "ArnNotLike": {
                    "aws:PrincipalARN": "arn:aws:iam::${Account}:role/[PRIVILEGED_ROLE]"
                }
            }
        }
    ]
}
```
---
2. `Deny Users from Modifying S3 Block Public Access:`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "s3:PutAccountPublicAccessBlock"
            ],
            "Resource": "*",
            "Condition": {
                "ArnNotLike": {
                    "aws:PrincipalARN": "arn:aws:iam::${Account}:role/[PRIVILEGED_ROLE]"
                }
            }
        }
    ]
}
```
---
3. `Deny Leave Organization`
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "organizations:LeaveOrganization"
            ],
            "Resource": "*"
        }
    ]
}
```
---

4. `Deny Service Access For Root User`
```json
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "RestrictEC2ForRoot",
        "Effect": "Deny",
        "Action": [
          "ec2:*"
        ],
        "Resource": [
          "*"
        ],
        "Condition": {
          "StringLike": {
            "aws:PrincipalArn": [
              "arn:aws:iam::*:root"
            ]
          }
        }
      }
    ]
  }
  
```