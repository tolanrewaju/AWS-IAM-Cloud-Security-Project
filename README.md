# AWS-IAM-Cloud-Security-Project
Cloud security controls in Amazon 
1. Project Overview
This project demonstrates how environment-based access control can be enforced in AWS using IAM policies. The task involved creating a least-privilege policy that controls EC2 instance start/stop operations based on tags. The policy was applied to an IAM group and validated by logging in as an IAM user and attempting various EC2 actions.
________________________________________
2. Tools & Concepts Used
•	AWS IAM — users, groups, policies, account alias
•	Amazon EC2 — tagging, lifecycle operations
•	IAM Policy Language (JSON) — Effect, Action, Resource, Condition
•	Cloud Security Principles — least privilege, access segmentation, policy testing
________________________________________
3. EC2 Tagging Strategy
To differentiate environments, each instance was assigned descriptive tags:
Instance	Tag Key	Tag Value
Marketing	Environment	Marketing
sales	Environment	Sales
These tags form the foundation for policy-level restrictions. 
And created buckets.
________________________________________
4. IAM Policy (JSON)
The custom policy below enforces restricted access to the Marketing instance while allowing full control on the sales instance.
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowStartStopForSalesInstance",
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Environment": "Sales"
        }
      }
    },
    {
      "Sid": "DenyStartStopForMarketingInstance",
      "Effect": "Deny",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Environment": "Marketing"
        }
      }
    }
  ]
}
________________________________________
5. AWS Account Alias
A custom sign-in alias was created to simplify access for IAM users and replace the long numeric login URL.
________________________________________
6. IAM Users & Groups
Steps carried out:
1.	Created an IAM group named Developers
2.	Attached the CybertechMarketingEnvPolicy to the group
3.	Added team members as IAM users with controlled EC2 permissions
________________________________________
7. IAM User Login Options
IAM users can authenticate via:
•	AWS Management Console (using the account alias)
•	AWS CLI (configured with Access Key ID & Secret Access Key)
________________________________________
8. Policy Testing & Validation
Real-world validation was performed by attempting EC2 operations as an IAM user.
Test Action	Expected Result	Actual Result
Stop Marketing instance	Denied	Access denied
Stop sales instance	Allowed	Successful
Start Marketing instance	Denied	Access denied
Start sales instance	Allowed	Successful
The test outcomes confirmed that the tag-based policy operates exactly as designed.
________________________________________
Project Structure (Recommended)
├── README.md
├── policy/
│   └── CybertechMarketingEnvPolicy.json
├── screenshots/
│   ├── ec2-tags.png
│   ├── account-alias.png
│   └── policy-test-results.png
________________________________________
Highlights
•	Enforced EC2 access segmentation using tags
•	Implemented custom least-privilege IAM policy
•	Validated permissions using multiple test scenarios
•	Improved user experience with an account alias
•	Documentation suitable for DevSecOps and Cloud Engineering portfolios
