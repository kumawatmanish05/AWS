# AWS IAM – Identity and Access Management

> A beginner-friendly guide to understanding, configuring, and using AWS IAM with practical examples.

## Table of Contents

1. [Introduction to IAM](#1-introduction-to-iam)
2. [Why Do We Need IAM?](#2-why-do-we-need-iam)
3. [IAM Functions](#3-iam-functions)
4. [Core IAM Components](#4-core-iam-components)
5. [How IAM Works](#5-how-iam-works)
6. [IAM Policies](#6-iam-policies)
7. [Types of IAM Policies](#7-types-of-iam-policies)
8. [Authentication vs Authorization](#8-authentication-vs-authorization)
9. [Important IAM Security Concepts](#9-important-iam-security-concepts)
10. [Practical Implementation](#10-practical-implementation)
11. [Using IAM with AWS CLI](#11-using-iam-with-aws-cli)
12. [IAM in Real-World Projects](#12-iam-in-real-world-projects)
13. [IAM Best Practices](#13-iam-best-practices)
14. [Common IAM Errors](#14-common-iam-errors)
15. [Interview Questions](#15-interview-questions)
16. [Quick Revision](#16-quick-revision)

---

# 1. Introduction to IAM

## What is IAM?

**IAM (Identity and Access Management)** is an AWS service that controls who can access AWS resources and what actions they are allowed to perform.

In simple words, IAM is like a security system for your AWS account.

It helps us:
- Create users and manage identities.
- Organize users into groups.
- Create roles for AWS services and applications.
- Assign permissions using policies.
- Control access to AWS resources.

### Real-Life Example

Imagine you are managing a company with three departments:

| Department | Required Access |
|---|---|
| Data Analyst | Read data from S3 |
| Developer | Deploy applications |
| Administrator | Manage AWS resources |

Should everyone have full access to AWS?

**No.**

A Data Analyst only needs permission to read data. A Developer needs permissions to deploy applications. An Administrator may need broader permissions.

IAM helps us control these different access levels.

## Important IAM Properties

- **Global Service:** IAM is a global AWS service. Its identities are not separately created for each AWS Region.
- **Free Service:** IAM is available at no additional charge.
- **Secure Access:** IAM helps protect AWS resources through identity and permission management.

---

# 2. Why Do We Need IAM?

Without IAM, managing access to AWS resources securely would be difficult.

Main reasons:

1. **Security:** Protect AWS resources from unauthorized access.
2. **Permission Management:** Assign permissions based on job responsibilities.
3. **User Management:** Create and manage different user identities.
4. **Group Management:** Manage permissions for multiple users together.
5. **Access Control:** Restrict access to specific AWS services.
6. **Account Protection:** Avoid using root credentials for daily activities.
7. **Cost Management:** IAM itself does not have an additional service charge.

### Example

A company has five employees:

- 2 Data Analysts
- 2 Developers
- 1 Administrator

Instead of giving everyone AdministratorAccess, we can create separate groups and assign permissions according to their responsibilities.

---

# 3. IAM Functions

AWS IAM provides the following functions:

| Function | Description |
|---|---|
| Create Users | Create identities for people or applications |
| Create Groups | Organize users into groups |
| Create Roles | Provide temporary access to trusted entities |
| Assign Permissions | Control which AWS actions are allowed |
| Manage Policies | Define permissions using JSON documents |
| Enable MFA | Add another layer of authentication |
| Review Access | Analyze and review access configurations |

---

# 4. Core IAM Components

IAM has four important components: Users, Groups, Roles, and Policies.

## 4.1 IAM Users

### What is an IAM User?

An IAM user represents an identity that can access AWS resources.

A user can have:
- Username
- Console password, if console access is enabled
- Access keys, if programmatic access is needed
- Permissions assigned directly or through groups

### Example

Suppose you have an employee named Rahul who works as a Data Analyst.

You can create an IAM user:

`rahul-analyst`

This user can receive permissions to access the company's data.

### When Do We Use IAM Users?

- When a person needs an individual AWS identity.
- When managing individual console access.
- When an application specifically requires IAM user credentials (although roles are generally preferred for workloads).

### How to Create an IAM User

1. Open the AWS Management Console.
2. Search for **IAM**.
3. Open the IAM service.
4. In the left navigation menu, select **Users**.
5. Click **Create user**.
6. Enter a username, for example `rahul-analyst`.
7. Enable console access if the user needs to log in through the AWS website.
8. Follow the password setup instructions.
9. On the permissions page, choose how you want to assign permissions.
10. Review the configuration and create the user.

**Result:** An IAM user has been created.

**Important:** Do not create access keys unless programmatic access is required. Never share passwords or access keys.

---

## 4.2 IAM Groups

### What is an IAM Group?

An IAM group is a collection of IAM users.

Instead of assigning identical permissions to each user individually, we can assign permissions to a group.

All users in the group inherit the group's permissions.

### Example

Suppose a company has three Data Analysts.

```text
             AWS IAM
                |
                |
          DataAnalysts
                |
       -------------------
       |        |        |
     User 1   User 2   User 3
```

If we assign read-only permissions to the `DataAnalysts` group, all three users receive those group permissions.

### When Do We Use Groups?

- When multiple employees need similar permissions.
- When managing permissions becomes difficult individually.
- When organizing users by job roles.

### How to Create an IAM Group

1. Open the AWS Console.
2. Search for IAM.
3. Select **User groups** from the left menu.
4. Click **Create group**.
5. Enter a group name:

   `DataAnalysts`

6. Under permissions policies, search for an appropriate policy.
7. For this practice, select `AmazonS3ReadOnlyAccess` if the goal is to read S3 resources only.
8. Click **Create group**.

**Result:** Your IAM group is created with S3 read-only permissions.

### How to Add a User to an Existing Group

1. Open IAM.
2. Select **User groups**.
3. Open `DataAnalysts`.
4. Find the Users section.
5. Click **Add users**.
6. Select the user you created.
7. Confirm the addition.

**Result:** The user inherits the permissions assigned to the group.

---

## 4.3 IAM Roles

### What is an IAM Role?

An IAM role is an identity with a set of permissions that can be assumed by a trusted entity.

Unlike normal long-term IAM user credentials, roles provide temporary security credentials.

### Why Do We Need Roles?

Imagine you have an EC2 instance running a Python application.

The application needs to download a dataset from an S3 bucket.

Should we store AWS access keys directly inside the Python code?

**No.**

Instead, we can create an IAM role and attach the required S3 permissions.

The EC2 instance assumes the role and receives temporary credentials.

### How Does It Work?

```text
       EC2 Instance
             |
             |
       Assumes IAM Role
             |
             |
       Temporary Credentials
             |
             |
          S3 Bucket
             |
             |
       Reads Dataset
```

### How to Create an IAM Role

Example: Create a role for an EC2 instance to read data from S3.

1. Open the IAM Console.
2. Select **Roles**.
3. Click **Create role**.
4. Under Trusted entity type, choose **AWS service**.
5. Select **EC2** as the use case.
6. Click Next.
7. Search for `AmazonS3ReadOnlyAccess`.
8. Select the policy.
9. Click Next.
10. Enter the role name:

    `EC2-S3-ReadOnly-Role`

11. Review and create the role.

### How to Attach the Role to EC2

1. Open the EC2 Console.
2. Select your EC2 instance.
3. Open **Actions → Security → Modify IAM role**.
4. Select `EC2-S3-ReadOnly-Role`.
5. Save the changes.

**Result:** The EC2 instance can use the role's permissions to access S3 without storing permanent access keys on the instance.

Note: Creating and running an EC2 instance may incur charges. You do not need to launch one just to learn how to create a role.

---

## 4.4 IAM Policies

### What is an IAM Policy?

An IAM policy is a JSON document that defines which actions are allowed or denied on AWS resources.

Policies are used to manage permissions.

### Example

Suppose a Data Analyst needs to read files from an S3 bucket.

We can create a policy that allows the user to list and download files from a specific bucket.

### Basic IAM Policy Structure

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::my-data-bucket"
        }
    ]
}
```

### Explanation

| Element | Meaning |
|---|---|
| Version | Policy language version |
| Statement | Contains permission rules |
| Effect | Allow or Deny an action |
| Action | AWS operation being controlled |
| Resource | AWS resource affected by the rule |
| Condition | Optional condition for applying a rule |

### Important Policy Values

**Effect:**
- `Allow` – Gives permission.
- `Deny` – Denies permission.

**Action:**
- `s3:ListBucket` – Lists objects in a bucket.
- `s3:GetObject` – Downloads an object.
- `s3:PutObject` – Uploads an object.
- `s3:DeleteObject` – Deletes an object.

**Resource:**

An AWS resource is identified using an ARN (Amazon Resource Name).

Example:

`arn:aws:s3:::my-data-bucket`

---

# 5. How IAM Works

Let's understand how IAM controls access.

Imagine a user wants to download a file from an S3 bucket.

1. The user authenticates with AWS.
2. The user requests access to an S3 object.
3. AWS evaluates the applicable permissions.
4. If an applicable Allow grants access and no applicable explicit Deny overrides it, access is granted.
5. Otherwise, access is denied.

### IAM Permission Flow

```text
        IAM User
            |
            |
       Access Request
            |
            |
      Policy Evaluation
            |
       -------------
       |           |
     Allow        Deny
       |           |
       ▼           ▼
   Access       No Access
   Granted
```

**Important:** AWS authorization can involve identity-based policies, resource-based policies, permissions boundaries, service control policies, and other applicable controls. The diagram shows the basic learning model.

---

# 6. IAM Policies

IAM policies define permissions.

There are several important policy categories.

## 6.1 Identity-Based Policies

These policies are attached to IAM users, groups, or roles.

Example: Give a user permission to read objects from an S3 bucket.

## 6.2 Resource-Based Policies

These policies are attached directly to resources.

Example: An S3 bucket policy can allow a specific AWS account or role to access the bucket.

## 6.3 Trust Policies

A trust policy defines which entities are allowed to assume an IAM role.

Example: An EC2 service is trusted to assume a role created for EC2.

---

# 7. Types of IAM Policies

## 7.1 AWS Managed Policies

Policies created and maintained by AWS.

Examples:
- `ReadOnlyAccess`
- `AdministratorAccess`
- `AmazonS3ReadOnlyAccess`

## 7.2 Customer Managed Policies

Policies created and maintained by the AWS customer.

They can be reused across multiple users, groups, and roles.

## 7.3 Inline Policies

Policies directly embedded into a single user, group, or role.

They are useful when a permission should belong exclusively to that identity.

---

# 8. Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Verifies who you are. | Determines what you can access. |
| Identity verification. | Permission verification. |
| Example: AWS login. | Example: Permission to read an S3 bucket. |

**Easy way to remember:**

Authentication = Who are you?

Authorization = What can you do?

---

# 9. Important IAM Security Concepts

## 9.1 Principle of Least Privilege

Give users and applications only the minimum permissions required to complete their tasks.

Example:

A Data Analyst only needs to read S3 files.

Do not give that person permission to delete S3 buckets or manage IAM users.

## 9.2 Multi-Factor Authentication (MFA)

MFA adds an additional verification step during authentication.

For example:
- Password
- Authenticator application code or another supported MFA method

### How to Enable MFA for the Root User

1. Sign in using the AWS root user.
2. Open the account security credentials settings.
3. Find the Multi-Factor Authentication section.
4. Choose Assign MFA device.
5. Follow the instructions to configure a supported MFA method.
6. Verify the setup.

**Important:** Protect the root user with MFA. Never share its credentials or MFA codes.

## 9.3 Root User

The root user is the original identity created when an AWS account is registered.

It has extensive account-level privileges.

Best practices:
- Enable MFA.
- Avoid using it for everyday tasks.
- Never share root credentials.
- Use appropriate IAM identities for regular work.

## 9.4 Temporary Credentials

Roles provide temporary credentials to trusted entities.

They are generally preferred over permanent access keys for applications running on AWS.

---

# 10. Practical Implementation

In this section, we will create a simple company access-management setup.

## Practical Project: Create a Data Analyst IAM User

### Objective

Create:
- One IAM group.
- One IAM user.
- One permission policy assignment.

The user should be able to read files from S3 but should not be able to upload or delete files.

### Step 1: Open AWS IAM

1. Sign in to the AWS Management Console.
2. Search for `IAM`.
3. Open the IAM service.
4. You will see sections such as Users, User groups, Roles, and Policies.

### Step 2: Create a User Group

1. Open **User groups**.
2. Click **Create group**.
3. Enter:

   `DataAnalysts`

4. Under permissions policies, search for:

   `AmazonS3ReadOnlyAccess`

5. Select the policy.
6. Click Create group.

**Why this policy?**

It provides read-only permissions for supported S3 operations rather than granting broad access to every AWS service.

### Step 3: Create an IAM User

1. Open **Users**.
2. Click **Create user**.
3. Enter:

   `analyst-user`

4. Enable console access if required.
5. On the permissions screen, select **Add user to group**.
6. Select `DataAnalysts`.
7. Complete the creation process.

### Step 4: Verify the User

1. Open IAM → Users.
2. Select `analyst-user`.
3. Open the Permissions tab.
4. Confirm the user receives permissions through the `DataAnalysts` group.

### Step 5: Test Permissions

Sign in using the IAM user's console credentials, if you enabled console access.

1. Open S3.
2. Try viewing the buckets and objects.
3. Check whether the account can perform an upload or delete operation.

Expected:
- Read operations allowed where the policy and resource configuration permit them.
- Write and delete operations not allowed by the read-only policy.

**Note:** This test depends on the bucket's own permissions and other applicable policies. Do not use a real production bucket for experimentation.

### Expected Architecture

```text
                 AWS ACCOUNT
                      |
                      |
                 AWS IAM
                      |
               DataAnalysts
                      |
                      |
                analyst-user
                      |
                      |
          AmazonS3ReadOnlyAccess
                      |
                      |
                  S3 Bucket
                      |
                 Read Access
```

---

# 11. Using IAM with AWS CLI

## What is AWS CLI?

AWS CLI stands for **Amazon Web Services Command Line Interface**.

It allows us to interact with AWS services using terminal commands instead of the AWS Management Console.

Example:

Instead of opening the S3 Console, we can list buckets using:

```bash
aws s3 ls
```

### Step 1: Install AWS CLI

Download the official AWS CLI installer:

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

For Windows, download and install the MSI package.

Verify the installation:

```bash
aws --version
```

### Step 2: Configure AWS CLI

For an IAM Identity Center setup, use the appropriate SSO configuration.

For a learning environment using IAM user access keys, configure the CLI only if programmatic access is necessary and permitted by your account's security policy.

```bash
aws configure
```

It asks for:

- AWS Access Key ID
- AWS Secret Access Key
- Default region
- Default output format

Never paste these credentials into GitHub, chat messages, or source code.

### Step 3: Check AWS Identity

```bash
aws sts get-caller-identity
```

This command shows which AWS identity the CLI is currently using.

### Step 4: List S3 Buckets

```bash
aws s3 ls
```

This lists S3 buckets that the current identity is allowed to list.

### Important CLI Commands

| Command | Purpose |
|---|---|
| `aws --version` | Check CLI installation |
| `aws configure` | Configure credentials and defaults |
| `aws sts get-caller-identity` | Check current identity |
| `aws iam list-users` | List IAM users, if permitted |
| `aws iam list-groups` | List IAM groups, if permitted |
| `aws s3 ls` | List accessible S3 buckets |

**Note:** AWS CLI commands require appropriate permissions. Do not create long-term access keys just to practice basic IAM concepts.

---

# 12. IAM in Real-World Projects

## Data Engineering

An AWS Glue job can use an IAM role to:
- Read raw datasets from S3.
- Process data.
- Write transformed datasets to another S3 location.

## Data Science

A machine learning application running on EC2 can use an IAM role to:
- Access training datasets from S3.
- Store model artifacts.
- Retrieve required AWS resources.

## MLOps

A CI/CD pipeline can assume an IAM role to:
- Deploy applications.
- Access deployment resources.
- Publish artifacts.
- Avoid storing permanent AWS credentials in source code.

## Example: ML Pipeline

```text
        AWS S3
    Training Dataset
           |
           ▼
      EC2 Instance
           |
      IAM Role
           |
           ▼
     Python ML Model
           |
           ▼
     Trained Model
           |
           ▼
        AWS S3
    Model Artifacts
```

IAM ensures that the application has only the permissions required for its job.

---

# 13. IAM Best Practices

1. Enable MFA for the root user and privileged identities.
2. Follow the Principle of Least Privilege.
3. Avoid using the root user for everyday tasks.
4. Prefer IAM roles for AWS workloads.
5. Avoid hardcoding access keys in Python files.
6. Never commit AWS credentials to GitHub.
7. Review user permissions regularly.
8. Remove unused credentials and identities.
9. Use managed policies or carefully scoped custom policies.
10. Use IAM Access Analyzer to review access configurations.

---

# 14. Common IAM Errors

| Error | Possible Reason | Solution |
|---|---|---|
| AccessDenied | Missing permission | Review applicable IAM and resource policies |
| InvalidClientTokenId | Invalid or incorrect access key | Check credential configuration |
| ExpiredToken | Temporary credentials expired | Obtain fresh credentials |
| User is not authorized | Required action is not allowed | Review the relevant policy |
| Unable to access S3 | Bucket or identity permissions are insufficient | Check IAM and bucket policies |

---

# 15. Interview Questions

### Q1. What is AWS IAM?

IAM is an AWS service that manages identities and controls access to AWS resources through permissions and policies.

### Q2. What is the difference between an IAM user and an IAM role?

An IAM user is an identity that can have long-term credentials. An IAM role is assumed by a trusted entity to obtain temporary security credentials.

### Q3. What is an IAM policy?

An IAM policy is a JSON document that defines which AWS actions are allowed or denied on specific resources.

### Q4. What is the Principle of Least Privilege?

It means giving an identity only the minimum permissions required to perform its task.

### Q5. What is the difference between an IAM group and a role?

A group organizes IAM users and shares permissions with them. A role is assumed by a trusted entity to obtain temporary credentials.

### Q6. What is MFA?

Multi-Factor Authentication adds an additional verification step to improve account security.

### Q7. Is IAM a Regional or Global service?

IAM is a global AWS service.

### Q8. Is AWS IAM free?

IAM is available at no additional charge.

### Q9. Why should we avoid using the root user?

The root user has extensive account-level privileges. Using it for everyday operations increases the risk of accidental or unauthorized changes.

### Q10. Why are IAM roles preferred for EC2 applications?

They provide temporary credentials and avoid the need to store long-term access keys on the instance.

---

# 16. Quick Revision

| Concept | Remember |
|---|---|
| IAM | Manages AWS identities and permissions |
| User | Individual identity |
| Group | Collection of IAM users |
| Role | Assumed identity with temporary credentials |
| Policy | Defines permissions |
| MFA | Extra authentication security |
| ARN | Amazon Resource Name |
| STS | Security Token Service |
| Least Privilege | Minimum required permissions |
| AWS CLI | Interact with AWS through terminal |

### Final Summary

AWS IAM is responsible for identity and access management in AWS.

- Users represent identities.
- Groups organize users.
- Roles provide temporary access.
- Policies define permissions.
- MFA adds security.
- Least Privilege reduces unnecessary access.

---

# 17. Learning Checklist

### Theory
- [ ] Understand AWS IAM.
- [ ] Understand IAM Users.
- [ ] Understand IAM Groups.
- [ ] Understand IAM Roles.
- [ ] Understand IAM Policies.
- [ ] Learn Authentication vs Authorization.
- [ ] Understand MFA.
- [ ] Understand Least Privilege.

### Practical
- [ ] Open AWS IAM Console.
- [ ] Create an IAM Group.
- [ ] Attach an appropriate managed policy.
- [ ] Create an IAM User.
- [ ] Add the user to the group.
- [ ] Verify inherited permissions.
- [ ] Understand how to create an IAM Role.
- [ ] Understand AWS CLI configuration.
- [ ] Run `aws sts get-caller-identity`.

---

## Next Topic: AWS CLI (Command Line Interface)

**Learning Goal:** Learn how to interact with AWS services using terminal commands and Python's Boto3 SDK.
