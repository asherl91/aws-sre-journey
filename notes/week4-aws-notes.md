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

