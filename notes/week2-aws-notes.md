Week 2 Day 6
Launched first EC2 instance, messed around a bit. Nothing big.

----
Week 2 Day 7

S3 Basics

S3 is a global storage platform. Runs on all regions, can be accessed from anywhere with internet. It's regional based because data is stored in a specific region when its not being used. Regional based/resilient. Never leaves that region unless you configure it to. Regionally resilient, replicated across AZ in that region. Can tolerate AZ failure.

S3 is a public service, so it can be accessed from anywhere with internet. Service runs on AWS public zone. Can cope with unlimited data amounts and designed for multi user usage of that data. 1mil users ok.

S3 perfect for hosting large amounts of data. 

Economical and accessed via UI/CLI/API/HTTP

S3 is default storage service in AWS. Default starting point.

S3 delivers two main things:
objects and buckets

Objects are the data it stores, pics, movies, datasets, etc.

Buckets are containers for objects.

Objects:
Objects are like files. Madeup of two main components and metadata. 
Object key: similar to file name. Identifies a file in a bucket.
Value: data/contents of the object. 
Value of an object (how large an object is) can range from 0 bytes to 5TB.

other smaller components:
version id
metadata
access control
subresources

Buckets:
Buckets are created in a specific AWS region. Impacts:
1. Data inside bucket has a primary home region. Never leaves unless you/someone configures that data to leave that region. S3 has stable and controlled data sovereignity. Blast radius = region. 

Bucket is identified by its bucket name. Bucket name must be globally unique, meaning one use, no one else can across all of AWS.

Buckets can hold unlimited number of objects, and due to size range, buckets can hold from 0 to unlimited bytes.

As an object storage system, an S3 bucket has no complex storage structure. Flat, all objects are on the same level. No levels or folders. UI will present itself as folders, but that's not how it actually works. If you add a folder type name to the object key, S3 will represent it under a folder, but its just a UI trick, not actually using folders. They are more like prefixes.

**Exam tips:**
Buckets are globally unique.

Bucket names need to be between 3-63 characters, all lower case, no underscores
Need to start with a lowecase letter or number
Can't be IP formatted
Buckets have 100 soft limit in AWS account. Hard limit of 1000. Can increase with support requests, but default is 100.

Bucket can be divided using prefixes to assign one bucket to multiple users, as the prefixes can sort of organize data.

Unlimited objects in a bucket, 0 to 5tb per object

Object Key = Name, Object Value = Data

***

S3 is an object storage system, its not a file or block storage system.
Meaning: if you have a req where you're accessing the whole of an object (img, audio, etc), then that is a cnadidate for obj storage.

If you have a windows server that needs to access a network file system, dont use s3. That needs to be file based storage. S3 has no file based storage.

Its not block storage, meaning it can't be mounted. Instances/VMs mount block storage, which are virtual hard disks. EC2 has EBS for that. 

S3 is great for large scale data storage, distribution, or upload. Great for offloading as well.

Shrink instance data usage onto S3 as well

S3 should be your default thought for any input to AWS or output from AWS. Most services which consume data and or output data can have S3 as an option to take data from or put data to when its finished.

Exam questions: S3 should be default if there's a number of answers of where to put data. 

---
Week 2 Day 8

Cloud Formation Basics

Cloud Formation is a tool that lets you create, updte, delete infrastructure in AWS in a consistent and repeatable way using templates. So you can create a template and use it again to create infrastructure.

Cloud Formation is written in either YAML or JSON.

Parts of a template for CloudFormation

-Resources:
All templates have a list of resources, at least one. It's the resources section that tells cloud formation what to do. Resources added, cloudformation creates resources. If updated, CF updates them. If resources are removed, they will be deleted by CF.

Resources is the only mandatory part of a template for CF.

Description:
free text field where you can add a description. Add details about the template. 

Restriction: if you have both a description and an aws tempmmlate format version, the description needs to follor the template format version. template format version is not mandatory, but if used, next up needs to be description

Template format version: 
the way that aws allow for extending the standards over time.

Metadata:
can control the different things in CloudFormation template are presented through the console UI/AWS UI. Forces how the UI presents the template. Also used for other things

Parameters:
Adds fields that prompt the user for additional info. Size of instance, name of somethjing, how many AZs to use, etc.
Under parameters:
-LatestAmiId: 
--Type, special type. allows us to instead of provide a specific ami id, we can tell it we want latest ami for a given distro.

-SSHandWebLocation: The IP address range that can be used to SSH to the EC2 instances

Mappings:
allows you to create lookup tables. will be explained later.

Conditions:
Allow decision making in the template. Set certain things that will only happen if a condition is met. If then essentially. Create the condition under conditions, Use condition under Resources.

Outputs:
a way that once the template is finished, it can present outputs based on whats being created, updated, or deleted. Kind of a results screen.
-Examples of Outputs: InstanceId, AZ, PublicDNS, PublicIP.
-Uses cloudformation functions. example: !ref, references another part of the template. !GetAtt, refers to another thing in the template, but can pick from different data. 

LatestAmiId:


How does CloudFormation use templates

Resources section is a list of resources. Resources inside a template are called logical resources. Instance is a logical resource, for creating an EC2 instance.
Under Instance (Logical Resource):
Type: AWS::EC2::Instance, telling it what to create
Properties: used to configure the resources in a certain way

CloudFormation creates a stack from a template. Can create multiple from one template. It is created when CF grabs a template.

For any logical resources, CF makes a corresponding physical resource in your AWS account. So if a resource is an EC2, physical resource is the EC2 instance.

So template creates a stack, then stack creates physical resources. If you update that template, the template will then update the stack and its logical resources, then make necessary changes to the physical resources. Add new ones, update others, delete some. If you remove a resource from a template and use it as an update, it will be removed from stack + physical.

Deleting a stack means logical resources are deleted, which causes CF to delete matching physical resource.

CF lets you automate infrastructure.

Demo:

-When uploading a template, it uploads it to an S3 bucket that it creates automatically. Might create multiple buckets with prefix CF

---

Week 2 - Day 10 (SKIPPED DAY 9 CAUSE IM BABY)

CloudWatch

Cloudwatch: core product in AWS. Suppport service used by almost all other AWS services for operational management and monitoring.

Cloudwatch collects and manages operational data on your behalf. Operational data: Details how it performs, how it normally runs, or any logging data it generates. 

Three main products in one.

Cloudwatch itself: Collects Metrics, monitors metrics, and performs actions based on metrics
Metrics: data related to AWS products, apps, or on-premises instances. Disk usage, cpu ultilization, etc.

Cloudwatch is a public service, can be used either within AWS or other cloud platforms. You can also use it from anywhere with permissions.

Some metrics are gathered natively by Cloudwatch. Anything running within AWS can be gathered natively.

Cloudwatch Agent: sometimes needed for some types of metric collection. Collect metrics outside of AWS, other cloud environemtns. Monitoring some things inside products that aren't exposed to AWS needs agent as well. Such as monitoring processes running in an EC2 instance.

Cloudwatch gathers and stores this data, and you can access the data via UI or other means.

Cloudwatch Logs: allows for collection, monitoring, and actions based on Logging data. Could be windows event logs, server logs, mainly anything that is logs.

Agent works the same here.

Cloudwatch events: functions like an event hub. services and schedules. If an AWWS service does something (terminated instance for example), events will generate an event that will perform another action. It can also generate an event to do something at a certain time.

Kind of like cpu-z/task manager stuff + windows event log + scheduler

Cloudwatch manages alot of stuff, so it needs a way to keep things separated.

Namespace: 
container for monitoring data. 
Has a name, can be anything, except AWS/:
All AWS data goes into an AWS namespace which is called AWS/servicename, such as AWS/EC2. This contains all metric data for EC2.
Namespace contain related metrics.

Metrics: collection of related data points in a time ordered structure. Ex. CPU util, network in/out, disk util. Time ordered: starts when you enable monitoring, finish when you disable it.
A metric is not for a specific server. CPU utilization is the metric. That metric can be receiving data from multiple ec2 instances.

Datapoints:
Everytime something reports its metric, the measurement reported is a datapoint. Datapoint in its simplest form consists of two things:
Timestamp:full date and time
Value: measurement recorded

Dimensions:
Dimensions separate datapoints for different things or pesrspectives withing the same metric. Metrics are being sent at the same time, so it includes all metrics for all available points. Dimensions separate that.

AWS sends InstanceID and InstanceType as dimensions
Namespace AWS/EC2 -> Metric CPU Util -> Dimensions for specific instance ID.

Alarms: Cloudwatch allows us to take actions based on those metrics. Created and linked to a metric. Configure the alarm to take an action based on that metric. If all good, then alarm is ok. But if condition triggered, then alarm goes into alarm status. Action is then triggered. Third state possible: insufficient data. Usual start status until it has enough data to choose OK or Alarm status.

Demo!

---
Shared Responsibility Model
:how AWS provide clarity around which areas of systems security are theirs, and which are owned by the customer.

AWS responsible for security OF the cloud
Customer is responsible for security IN the cloud.

AWS is responsible for managing the security of the AWS regions, AZs, edge locations, the hardware and security of the global infrastructure. Same with Compute, Storage, Database and Networking services AWS provides us. Any software which assists in those services as well is managed.

Customer responsibility:
Client side data encryption, integrity and authentication, serverside encryption (file system and/or data), networking traffic protection (encryption, integrity, identity)
OS, network, firewall configuration, applications, paltform, identity and access management, customer data.

---
High Availability vs Fault Tolerance vs Disaster Recovery
High Availability: HA
Fault Tolerance: FT
Disaster Recovery: DR

HA:
Aims to ensure an agreed level of operational performance, usually uptime, for a higher than normal period.
HA isn't aiming to stop failure. outages are still probable.
An HA system is designed to be online and provide services as often as possible. If fails, its components can be replaced or fixed as quickly as possible often using automation.

Not about user experience. If something fails and disrupts usage for a few seconds, thats okay. Still HA. Maximizes systems online time.

System availability is expressed of percentage of uptime:
99.9% (three 9s) = 8.77 hours of downtime per year
99.999% (five 9s)= 5.26 minutes of downtime

FT:
similar to HA, but more.
Property that enables a system to continue operating properly in the event of the failure of some (one or more faults within) of its components.

If a system has faults, it should continue to operate properly while its being fixed, without impacting customers.

FT = operate through failure. can be expensive since it is complex to implement.

DR:
a set of policies,  tools and procedures to enable the recovery or continuation of vital technology infrastructure and systemns following a natural or human induced disaster.

What to plan for and do once disaster occurs which knocks out a system. HA/FT doesn't work, DR comes in. fire/floods, etc.
Pre-planning -> DR Process
Pre plan for everything in advance, staffing and physical issues.
For example, have a standby premises in case of disaster.
Resilience for local infrastructure, like VM or extra hardware at backup site.
Regular backups, do not store them at the same site of the system. Backups should be offsite.
Copies of processes, logins, etc.

Periodic DR testing

HA: minimize outages (user downtime is okay)
FT: operate thorugh faults
DR: used when HA/FT don't work, how we recover

AWS helps with all of these.