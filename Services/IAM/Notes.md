### 1. What is IAM ?
IAM (Identity and Access Management) is an AWS service that controls who can access AWS resources and what actions they are allowed to perform.

### 2. Why do we use IAM?
- Manage access to AWS services securely.
- Give users only the permissions they need.
- Control access to resources such as S3 buckets, EC2 instances, and databases.
- Avoid sharing the AWS root account.
- **Free Service** - IAM is offered to no additional cost
- Global Service

### 3. IAM Function 
AWS IAM provides these main functions to securely manage access to AWS resources.

- **Create Users**- You can create individual user accounts for people who need access to your AWS resources.
- **Assign Permissions**- You can assign specific permissions to users,groups, or roles to control what actions they can perform on AWS services.
- **Create Groups**- You can group users together and assign permissions to the group, making managemet easier for multiple users.

### 4. Roles
- **Create Roles**- You can create roles to assign temporary permissions to AWS services or users , esplically useful for securely managing permissions across different AWS resources .
- **Define Policies**- You can create and attach custom policies to define fine-grained permissions for controlling access to AWS resources.
- **Manage Federated Access**- IAM allows integrating with external identity providers(like Active Directry) for centralized management of user access across AWS.

### 5. MFA 
MFA(Multi -Factor authentication) is an extra layer of security that requires users to provide two or more forms of verification , like a password and a code from their phone , to access their accounts.

**MFA** = **Username+Password+Security_code**

### 6. Different Ways of accessing AWS 
- **AWS Management Console**: A web-based graphical interface used to manage AWS services.
- **AWS CLI**: A command-line tool used to access and manage AWS services through commands.
- **AWS SDK**: A set of libraries used to interact with AWS services through programming languages like Python (Boto3).
- **AWS APIs**: Interfaces that allow applications to communicate directly with AWS services.
- **AWS CloudShell**: A browser-based terminal used to execute AWS CLI commands without local installation.
- **AWS Mobile Application**: A smartphone application used to monitor and manage selected AWS services.


