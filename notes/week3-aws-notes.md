Week 3

# Day 13 - IAM

## IAM Policies

IAM policies are policies which get attached to identities inside AWS. Identities are IAM users, groups and roles. 

IAM policy is a set of security statements to AWS. Grants/denies access to AWS products/features to any identity who uses that policy. 

Identity policies/policy documents are created with JSON.

High level: policy document is just 1 + statements. Each statement grants/denies permissions to AWS services.

Authentication: When an identity attempts to use/access AWS resources, it needs to prove who it is to AWS. Once authenticated, that identity is now authenticated identity.

AWS then knows what policies each identity has, can be multiple.

With the exception of account root user, aws identities start off with no access to any aws resources.

## Parts of a statement

Statements are what allows/denies actions in a policy

#### SID
SID = Statement ID
Optional field, lets you identify a statement/what it does. title/description sort of. Best practice to always use.

#### Action
Every interaction you have with AWS is a combination of the resource you're interacting with and the actions you are attempting to perform on that resource.

A statement only applies if the interaction you're having with AWS match the action and the resource.

The action part of a statement matches one or more actions. It can be specific, listing a specific single action. Or you can use wildcards to match any operation. Could also be a list of individual actions.

#### Resource
Similar to action, but matches AWS resources. Wildcards can be used, specify individual AWS resources, or specify lists of AWS resources.

#### Effect
Allow or deny. Effect controls what AWS does if action+resource match the operation you're attempting to do with AWS. So if action is access bucket, resource is s3, and you are trying to access s3, effect gets checked: allow or deny.

## Allow and Deny in same Policy

Basically, there could be overlaps in statements. S3 allow all actions, but another denying a specific bucket.

One rule to assist:
DENY - ALLOW - DENY

First priority:  explicit denies
If you have a statement that explicitly denies something, that is highest priority, overrules everything else. 

Second priority: explicit allows
These are allowed unless there is an explicit deny.

If no statement exists for what you are trying to do, default deny (implicit). So without an explicit allow, you can't do anything.

DENY - ALLOW - DENY

If multiple policies attached to action, they are all read at the same level and still, DENY - ALLOW - DENY. One explicit deny, thats all thats needed.

## Types of policies

Inline policies and managed policies.

Same thing, JSON policy documents. Difference is how they're managed.

Inline policy: assigning a new policy to individual accounts, one at a time. One JSON/policy per identity, added individually, not synced. Works, but not best practice due to multiplicity.

Managed Policy: created as their own object. Create one policy, then you can attach it to each needed identity. Since it is one policy being attached, updating it would update all using it. Reusable and low management overhead. Managed policy works great for common access for generic identities/deafult access.

When to use inline policies? exceptions. Special or Exceptional allow/deny.  

Two main types of managed policies: created and managed by AWS.

Customer managed policies: you can create them custom for your needs.

## IAM Users

IAM users are an identity used for anything requiring long-term AWS access, like humans, apps, or service accounts.

IAM starts with a principal: an entity trying to access an AWS account. At start, unidentified.

Principals can be individual peoples, computers, services, groups of any of these. For a principal to be able to do anything, it needs to be authenticated or authorized.

A principal makes requests to IAM to interact with resources. To do this, it needs to authenticate against an identity with IAM.

Authentication is a process where the principal proves to IAM that its the identity it claims to be. 

Authentication is done either user/pass or access keys. Both are long term credentials. Human = user/pass. Application or human using CMD = access key.

Authorization: Once they authenticate, principal is known as authenticated identity. It then knows which policies/statements to apply to that identity. This is authorization, adding the statements to that identity once the principal/identity is authenticated. When the identity tries to do something, AWS checks to see if its authorized to do so via statements/policies.

## ARN - Amazon Resource Names

ARNs uniquely identify resources within any AWS accounts. Helps identify specific resources in an unambigious way. 

ARMs are collections of fields split by a colon. double colon means nothing in between it, skipped. 

First field is partition, partition resource is in. For standard aws regions, partition is aws. if you have resources in other partitions, this would be arn-partitionname. Usually always AWS. If you have resources in another region, it would be slightly different. AWS-cn for example for china AWS.

Next is service, service name space that identifies the AWS product. S3, IAM, RDS, etc.

Region next, region the resource you are referring to resides in. Some arns do not require a region, like s3, so it might be omitted, or certain ones require a *.

Next is Account ID, account id of the aws account that owns the resource. if you are referring to an ec2 instance in a specific account, you'll have to specify the account number inside the ARN. Some don't require it. 

Resource/resource type: varies depending on the service. resource can be name or ID of an object, could be a resource path.

## Exam Tip!

You can only have 500 max IAM users in a single account. IAM is global, so this is per account limit, not per region. 

IAM user can be a member of max 10 IAM groups.

This has sytems design impacts.

IF you have a system that requires more than 5000 identities, you can't use one IAM user per. Limit for internet scale applications or large organizations.

IAM Roles or Identity Federation fix this.

## Demo!

