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