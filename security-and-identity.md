# Security & Identity 

When design a cloud system, it is important to make sure that user who have access to the system only have access to the section that they absolutely need to have access to.

## Data Protection


| AWS Services | Function |
|---|---|
| Macie | Discover and protect sensitive data |
| Key Management Service | Store and manage encryption keys |
| CloudHSM | Hardware base key storage |
| Certificate Manager | Provision, manage, and deploy SSL and TSL security certificates |
| Secretes Manager | Rotate, manage, and retrieve secretes |


## Infrastructure Protection

| AWS Services | Function |
|---|---|
| Shield | Denial of service protection |
| Web Application Firewall | Filter malicious website traffic |
| Firewall manager | Centrally manage firewall rules |

## Thread Detection

| AWS Services | Function |
|---|---|
| Guard Duty | Atomically detect detect threats |
| Inspector | Analyze application security |
| Config | Record and evaluation of configurations of your AWS resources |
| Cloud Trial | Track user activity and API useage |

## Identity Management

| AWS Services | Function |
|---|---|
| IAM | Securely manage access to AWS account service and resources |
| Single Sign On | Implement cloud single sign on |
| Cognito | Manage identity inside applications |
| Directory Services | Implement and manage Microsoft Active Directory |
| Organizations | Centrally govern and manage multiple AWS accounts in one place |

## AWS IAM (Identity and Access Management)

This service manages the who can access what in an AWS account. In order to achieve this, it supports the use of creation of users and groups to either grant or deny access through the definition of user policy.

This service is free and in included in all AWS account.

### IAM Users

An IAM user is essentially a combination of username and password. When an AWS account is created, the owner of the AWS account is assigned a *Root User* account. That user can then define other users in the system, who can have access to different services in the system.

For example:

We can have user $\{A, B, C, D, E\}$ and services $\{\alpha, \beta, \gamma, \delta\}$.

We can define: 

* $A$ have access to $\{\alpha\}$ 
* $B$ have access to $\{\beta, \gamma, \delta\}$
* $C$ to have access to $\{\delta\}$
* $D$ to have access to $\{\alpha, \gamma\}$
* $E$ to have access to $\{\delta\}$

The scope of which services a user can access is defined using a *policy*.

To makes definition easier, we can assign users into groups and design a policy for that group

Example:

Notice that both $D$ and $E$ have the identical access. Therefore, we can define group $\{D, E\}$ have access to $\{\delta\}$

#### Policy Definition

Policy can be defined in a json file. For example

```json
{
    "Version": "2019-10-17",
    "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": "*",
                        "Resource": "*"
                    }
                ]
}
```

The policy defined above gives a user the administrative access to an account. `"Effect": "Allow"` grants the user access, `"Action": "*"` grants the user to use all possible action, and `"Resource": "*"` grant the user access to everything. Therefore, the user can do everything to everything in that account.

Another Example:

```json
{
    "Version": "2019-10-17",
    "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": [
                                    "s3: ListBucket"
                                ],
                        "Resources": "arn:aws:s3:::bucket-name"
                    },
                    {
                        "Effect": "Allow",
                        "Action": [
                                    "s3: PutObject",
                                    "s3: GetObject"
                                ],
                        "Resources": "arn:aws:s3:::bucket-name/*"
                    }
                ]
}
```

In the above example, the user have access to the storage service `S3`. In there, `s3:ListBucket` lets the user to see the buckets and `"s3: PutObject"` and `"s3: GetObject"` get the user read and write data to the storage service. and `"Resources": "arn:aws:s3:::bucket-name/*"` give access to everything in the bucket.

### IAM Roles

Roles allows to delegate access to user are a service. For a role, not a user can assume a role so as a service. For example, in an account, there is a virtual machine running a website and a database. We can in this case, create a role to allow some user to access the database. But the virtual machine can also assume to role and be granted access to the database. 

This provides an extra level of security from the usual username and password and firewall. 

Additionally, role can grant access user from different AWS account to use services from other AWS account. 

### Secrets Manager

Historically, when a website needs to access a database, the password to that database has to be hard coded into that website. This introduces all kinds of problem.
 
The AWS secrets manager is a service that protects the secrets required to access resources. It can automatically rotates the secrets. To use it, we can code a query in the codebase to request the secrets

```python
    password = get_secret_value_response["SecretString"]
```

In addition to that, the secrets manager can also change the password according to a set interval. 

## Directory Service

When an organization manages its users login. It uses a directory, where all the user names and passwords are listed. When a username and a password is typed in, they are checked against the directory to determine the eligibility of access. This can also be extended with function such as the automatic configuration of environment.

The AWS Directory Services is a managing service that provides the directory. It can produce:

* A managed Microsoft Active Directory
* A managed Simple Active Directory
* AD connector (Allow on premise user to log into AWS with their active directory credential)
* Distributed services with automatic failover
* With Microsoft Directory managed by AWS will also have access to other AWS services. 