# Cloud Security with AWS IAM

## Project Overview

This project demonstrates how to securely manage access to AWS resources using **AWS Identity and Access Management (IAM)**. The focus is on setting up IAM users, user groups, and policies to control access to EC2 instances based on environment tags, ensuring proper security for different AWS services.

## Key Features

- **User Management:** Created and managed IAM user accounts with distinct credentials.
- **User Groups:** Defined user groups with shared permissions to streamline access control.
- **IAM Policies:** Set up JSON-based policies to precisely define permissions for AWS resources.
- **Tags for Resource Management:** Used tags (`Env = production` and `Env = development`) to organize EC2 instances and control access.
- **Policy Testing:** Validated the policy setup by testing access control for EC2 instances based on their tags.

## What is AWS IAM?

**AWS Identity and Access Management (IAM)** allows you to manage access to AWS resources securely. You can create and manage AWS users, groups, and roles, and control permissions using policies. This ensures only authorized users and systems can access specific resources.

## IAM Policy Details

The project’s key policy focuses on managing EC2 instances tagged with `Env = development` and restricting access to instances tagged with `Env = production`.

### JSON Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:DescribeInstances"
            ],
            "Resource": [
                "arn:aws:ec2:region:account-id:instance/*"
            ],
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Env": "development"
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],
            "Resource": "arn:aws:ec2:region:account-id:instance/*"
        }
    ]
}
```
## Policy Breakdown
- Effect: Determines whether the action is allowed or denied.
- Action: Specifies which actions (e.g., StartInstances, StopInstances, DescribeInstances) are controlled.
- **Resource:** Defines which EC2 instances the policy applies to.
- **Condition:** Ensures that the actions are only applied to instances tagged with `Env = development`.

## Tags

Tags are metadata labels assigned to AWS resources, helping organize and manage them. In this project, the following tags were used:
- **Env = production**
- **Env = development**

These tags help control permissions based on the environment (e.g., allowing actions only on development instances).

## Account Alias

An **AWS Account Alias** was created to replace the default AWS numeric ID, making the sign-in URL more user-friendly:  
`https://alias-sanil.signin.aws.amazon.com/console`

## Testing IAM Policies

After creating the policy, I tested it by:
1. **Stopping the production instance:** As expected, I received an `Access Denied` error, confirming the policy was correctly applied.
2. **Stopping the development instance:** This action was successful, indicating that the permissions were set correctly.

## What I Learned

This project taught me the intricacies of configuring IAM policies. The balance between security and functionality requires attention to detail, especially when defining permissions and testing them in real-world scenarios.

## Conclusion

This project demonstrates how AWS IAM can be used to manage and secure access to resources in a cloud environment. By leveraging IAM policies, tags, and user groups, organizations can ensure proper access control while maintaining a secure and organized cloud infrastructure.
