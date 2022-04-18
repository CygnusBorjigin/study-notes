# IAM Lab

The IAM is a global service of AWS, which means the groups and permissions set up on this service is applicable across all regions. However, in more advance use case, it is possible to set constrains on users from specified regions. 

In the user section, we can view the information of all the user in the account. Each user has an amazon resource name (ARN). 

By definition, all services are denied to all users in an account. A resource is granted to a user only if all the policy apply to that user grants that user access to the resource. 