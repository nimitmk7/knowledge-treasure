IAM stands for Identity and Access Management. 

## Users
An _IAM user_ is an entity that you create in your AWS account. The IAM user represents the human user or workload who uses the IAM user to interact with AWS resources. A IAM user consists of a name and credentials.

### Roles 

An IAM _role_ is an IAM identity that you can create in your account that has specific permissions. 

An IAM role is similar to an IAM user, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. 

However, ==instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it==. 

Also, a role does not have standard long-term credentials such as a password or access keys associated with it. Instead, ==when you assume a role, it provides you with temporary security credentials for your role session==.

You can ==use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources==. 


