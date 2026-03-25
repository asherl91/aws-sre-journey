Week 4

# Day 17

## CloudWatch Logs

CloudWatch Logs is a service which can accept logging data, store it, and monitor it. Logs will be used in CloudTrail, coming up next.

Cloudwatch Logs product allows you to store, monitor, and access logging data.

Logging data is a piece of info data and a timestamp. 

Cloudwatch Logs is a public service, hosted in the AWS public zone. Can be used within AWS VPCs, onpremises environments, or other cloud platforms as long as you have network connectivity and permissions.

CW Logs has built in integrations with AWS services:
EC2, VPC Flow Logs, Lambda, CloudTrail, R53, more...
Any of these can store data directly inside CW Logs
Security for this is provided by IAM roles or service roles.

For anything outside of AWS or for logging custom application or OS logs on EC2 for example, you can use the unified CW agent. 

Additional way: dev kits for AWS and implement CloudWatch logs directly into your app.

Metric filters: CW Logs are also capable of taking logging data and generating a metric from it = Metric Filter.

CW = regional service. 

Log Events: Logging sources inject data into CW logs as log events. Timestamp -> Message block, treated as raw block of data.

Log Streams: Log events are stored inside log streams. A sequence of log events from the same source. Each log stream is an ordered set of log events for a specific source for a specific thing.

Log groups: containers for multiple log streams for the same type of logging. Also stores configuration settings, such as retention settings and permissions. Metric filters are defined here as well. When a metric filter is detected, it increments a metric, and metrics can have associated alarms, which can be used to notify admins or to integrate with AWS/external systems to take actions.

# Day 17 Pt 2

## CloudTrail

Cloudtrail (CT) is a product which logs API calls and account events.

CT is a product which logs API actions which affect AWS accounts. Stopping an instance, change security group, create/delete s3 bucket, all logged.

CT Basics:
Product logs API calls or account activities, all logs are called as a CloudTrail Event.

CloudTrail Event: a record of an activity in an AWS account. This activity can be an action taken by a user, a role, or a service.

By default, CT stores the last 90 days of CT events in CT Event History. Area of CT enabled by default in AWS accounts. no cost for 90 day history. Customizing beyond 90 day history, you need to create a Trail.

CT Events can be one of three different types:
Management Events:
provide information about management operations that are performed on resources on your AWS account. Also known as control plane operations. Things like creating/terminating EC2 instance, creating VPC.

Data Events
contain information about resource operations performed on or in a resource. Things like objects being uploaded to S3, or access from S3, or when a lambda function is being invoked.

Insight Events - explained in separate video (maybe?)

By default, CloudTrail only logs management events because data events are much higher volume.

CloudTrail Trail (CT Trail) is the unit of configuration within the CT product. Its how you provide configuration to CT on how to operate. Trail logs events for the AWS region that it's created in.

CT is a regional service, but when creating a trail, it can be a one-region trail, or it can be set to all regions.

Single region trail is only ever in the region its created in. Only logs events for that region.

All region trail can be seen as a collection of trails in AWS region, but its managed as one logical trail. Additional benefit: if AWS adds any new regions, all region trail is auto updated.

Most services log events in the region where the event occurred. 

Global Service Events: A very small number of services log events globally to one region. Global services like IAM, STS, CloudFront, are globally focused services so they log their events to US EAST 1 (N. Virginia). A trail needs to have this enabled in order to log these events. This feature is normally enabled by default when creating a trail inside UI.

ONce trail is created, management and data events are all captured by the trail. Data events are not enabled by default. 

When you create a trail, you can be flexible on the 90 day history limit. A trail by default can store events in a definable S3 bucket. Logs which are generated in S3 bucket can be stored indefinitely. Only charged for the S3 storage used. They're stored as JSON so they're small, and can be parsed easily.

CT can be integrated with CloudWatch logs. CT can take all of hte logging data it generates, store it into S3 and CW Logs. Once in CW Logs, you can use CW logs to search through it or use a metric filter to take advantage of this data.

Recent addition to CT: creating Organizational Trails. If you create an organizational trail from the mgmt account of an organization, it can store all of the information for all of the accounts inside that organization. A single management point for all API and account events across all accounts in the organization. 

Recap:

-CT is enabled by default on AWS accounts, but only the 90 day event history. No storage in S3 unless you configure a trail.

-Trails are how you can get the data that CT has access to and store it in better places, like S3 and CW logs.

-Default for trails is to store maangement events only. Data events need to be enabled and come at an extra cost.

-Most AWS services log data to the same region that service is in, but a few are Global (IAM, STS, Cloudfront). They log their data as global service events, which gets logged to US EAST 1 and a trail will need to be enabled to capture that data.

-CT is not real time, there is a delay of about 15 minutes before the activity shows up. Publishes log files multiple times per hour

## Demo!

# Day 18

## AWS Control Tower

AWS Control Tower: at high level, control tower allows quick and easy setup of multi account environments. AWS does this, but control tower goes further and uses Organizations, along with IAM Identity Center (AWS SSO), CloudFormation, AWS Config, and more.

An evolution of AWS Organizations, adding more features, inteligence and automation.

Landing Zone - multi account environment. AWS Orgs with superpowers. Provides via other AWS services: SSO and ID Federation, centralized logging and auditing (Cloudwatch, Cloudtrail, AWS Config and SNS).

Control Tower provides guard rails - detect/mandate rules/standards across all accounts in the landing zone.

Account Factory: automates and standardizes new account creation.

Dashboard: single page oversigh of the entire organization

When control tower is first set up, it creates 2 OUs: 
foundational OU, called security by default. 
Custom OU, called sandbox by default.

### Foundational OU (Security):
In Foundational OU, Control tower creates 2 AWS accounts, audit account, and log archive account. 

Log archive account is for users that need access to all logging information for all of your enrolled accounts within the Landing Zone. AWS Config and CloudTrail Logs are stored within this account. Need to allow explicit access to this account and it offers a secure read only archive account for logging. 

Audit account is for users that need access to the audit information made available by control tower. Also can be used for any third party tools to perform auditing of your environment. SNS and CloudWatch will be used in this account.

### Account Factory/Custom OU (Sandbox)

Account Factory: similar to a team of robots who are creating/modifying/deleting AWS accounts as business needs. Can be interacted with via Control Tower console or Service Catalog.

Within Custom OU, Account Factory will create AWS accounts in a fully automated way, as many as needed. Config of accts is handled by Account Factory. Baseline templates applied for both account and network. Ensures consistency.

Control tower uses CloudFormation to implement this automation, so expect to see stacks created. 

Control tower also uses AWS Config and SCPs to implement account guardrails.

### Landing Zone

Landing Zone is a feature designed to allow anyone to implement a well architected multi account environment. Has the concept of a home region which is the region that you initially deploy the product into. (US EAST 1). You can explicityly allow/deny usage of other regions, but home region, the one you deploy into, is always available.

Landing Zone is built with AWS Organizations, AWS Config, CloudFormation, and more. Control Tower brings features of lots of AWS products together and orchestrates them. 

Aside from Security and Sandbox OUs, you can create other OUs and accounts.

Landing Zone utilizes the IAM identity center to provide SSO across multiple AWS accounts within the LZ and can also use ID Federation.

LZ provides Monitoring and notifications using cloudwatch and SNS

Can allow end users to provide new AWS accounts within LZ using Service Catalog

### Guard Rails

Guardrails are rules for multi account governance. 

Come in 3 different types:

Mandatory: always applied

Strongly Recommended: strongly recommended by AWS

Elective: implement niche requirements (fully optional)

Guard rails function in two ways:

Preventitive: stop you doing things within your AWS accounts in your landing zone. Implemented using SCPs. These are either enforced or disabled.
Example: allow/deny regions, disallow bucket policy changes

Detective: Compliance check. Uses AWS config rules and allows you to check that the config of a given thing within an AWS account matches what you define is best practice. 

These are either Clear, In Violation, or Not Enabled
Example: detective guard rail to check whether CloudTrail is enabled within an AWS account, or whether anyn EC2 instances have public IPv4 addresses associated with those instances.

Distinction: preventitive stop things from occurring, detective only identify those things.

### Account Factory Part 2

Automated Account Provisioning, can be done by cloud admins or end users with appropriate permissions. Guard rails are automatically added to automatically generated AWS accounts.

#### Confused here, a lot of notes

Cantrill: "Account admin given to a named user (IAM Identity Center) Because these accounts can be provisioned by end users, think of these as members of your organization, then either these members of your organization, or anyone that you define can be given admin permissions on an AWS account which is automatically provisioned. This allows you to have a truly self service automatic process for provisioning AWS accounts. So you can allow any member of your organization within tightly controlled parameters to be able to provision accounts for any purpose which you define as okay. and that person will be given admin rights over that aws account. These can be long running accounts or short term accounts."

"If I create a new account, why do I need an admin role?
Am I creating this account for another user?"
--Key Clarification
Creating an AWS account ≠ having access to it
Account creation is an organizational action
Access is always granted via roles

--What Account Factory Actually Does
When you create an account:

1. AWS creates a new account (isolated environment)
2. Control Tower assigns YOU an admin role in that account
3. You access it by assuming that role

--Why You Still Need to AssumeRole
Users exist in IAM Identity Center (central)
Accounts are separate environments

So:
You (Identity Center user)
→ AssumeRole → enter the account

This is always required—Control Tower does not change this.

--Who Is the Account For?
Most Common Case
You create the account for YOURSELF (your environment)

Example:
Dev account
Sandbox account
Other Case
You create the account for a TEAM

Then:
You assign other users roles into that account

--What “Admin Role” Actually Means
You are allowed to become admin inside the account

Not:

You permanently exist inside the account

--Important Constraint
Admin role is still limited by SCPs

So:

Admin ≠ full unrestricted access

--Final Mental Model
Accounts = environments
Users = people (in Identity Center)
Roles = access into environments
AssumeRole = how you enter

--One-Line Summary
Account Factory creates environments for you, then gives you a role to access them

Accounts can be long term or short term.

#### Back to Account Factory

Accounts are also configured with standard account and network configuration. If you have any organizational policies for how networking or any account settings are configured, these auto provisioned accounts will come with this configuration. Includes ip addressing used by VPCs.

Account Factory allows accounts to be closed or repurposed, and this whole process can be tightly integrated with the business's SDLC (software development life cycle).

