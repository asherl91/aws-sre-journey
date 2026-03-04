Animals4Life Architecture
Head office in Brisbane
On-premise: 192.168.10.0/24
AWS Pilot (bad): 10.0.0.0/16
Azure Pilot (bad): 172.31.0.0/16

Three major offices
London: EU region
New York: east coast
Seattle: west coast

Major offices consume services from Brisbane, low performance

---

AWS Basics

AWS account: container for identities (users) and resources.
Identities: correct way to refer to users

Root user has full access to everything, no restrictions

IAM: Identity and Access Management
Use to create users, groups, roles and apply permissions to.
IAM users start with no permissions by default

All AWS has IAM.
IAM globally resilient service, any data is always secure across all AWS regions. ->

IAM Users: humans or applications that need access to AWS account
IAM groups: collection of related users, team
IAM roles: Can be used by AWS Services, or for granting external access to your account. Used when granting access to services in account to an uncertain number of entities. 

IAM Policy: Allow or Deny access to AWS services when attached to users, groups, or roles. On their own do nothing. 

IAM has three main jobs:
IAM Manages Identities, An ID provider (IDP). Lets you create, modify, or delete identities such as users and roles

It also Authenticates the above, proves their ID. User/pass mainly

Authorizes, allows or denies access to resources when you are authenticated.

IAM is a Global Service/global resilience

IAM = Identity Federation and MFA

