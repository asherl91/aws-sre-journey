# Week 2 Day 6
Launched first EC2 instance, messed around a bit. Nothing big.

----
# Week 2 Day 7

## S3 Basics

S3 is a global storage platform. Runs on all regions, can be accessed from anywhere with internet. It's regional based because data is stored in a specific region when its not being used. Regional based/resilient. Never leaves that region unless you configure it to. Regionally resilient, replicated across AZ in that region. Can tolerate AZ failure.

S3 is a public service, so it can be accessed from anywhere with internet. Service runs on AWS public zone. Can cope with unlimited data amounts and designed for multi user usage of that data. 1mil users ok.

AWS Public Zone

S3 perfect for hosting large amounts of data. 

Economical and accessed via UI/CLI/API/HTTP

S3 is default storage service in AWS. Default starting point.

S3 delivers two main things:
objects and buckets

Objects are the data it stores, pics, movies, datasets, etc.

Buckets are containers for objects.

### Objects:
Objects are like files. Madeup of two main components and metadata. 
Object key: similar to file name. Identifies a file in a bucket.
Value: data/contents of the object. 
Value of an object (how large an object is) can range from 0 bytes to 5TB.

other smaller components:
version id
metadata
access control
subresources

### Buckets:
Buckets are created in a specific AWS region. Impacts:
1. Data inside bucket has a primary home region. Never leaves unless you/someone configures that data to leave that region. S3 has stable and controlled data sovereignity. Blast radius = region. 

Bucket is identified by its bucket name. Bucket name must be globally unique, meaning one use, no one else can across all of AWS.

Buckets can hold unlimited number of objects, and due to size range, buckets can hold from 0 to unlimited bytes.

As an object storage system, an S3 bucket has no complex storage structure. Flat, all objects are on the same level. No levels or folders. UI will present itself as folders, but that's not how it actually works. If you add a folder type name to the object key, S3 will represent it under a folder, but its just a UI trick, not actually using folders. They are more like prefixes.

## EXAM TIPS
Buckets are globally unique.

Bucket names need to be between 3-63 characters, all lower case, no underscores
Need to start with a lowecase letter or number
Can't be IP formatted
Buckets have 100 soft limit in AWS account. Hard limit of 1000. Can increase with support requests, but default is 100.

Bucket can be divided using prefixes to assign one bucket to multiple users, as the prefixes can sort of organize data.

Unlimited objects in a bucket, 0 to 5tb per object

Object Key = Name, Object Value = Data

## S3 Continued
S3 is an object storage system, its not a file or block storage system.
Meaning: if you have a req where you're accessing the whole of an object (img, audio, etc), then that is a cnadidate for obj storage.

If you have a windows server that needs to access a network file system, dont use s3. That needs to be file based storage. S3 has no file based storage.

Its not block storage, meaning it can't be mounted. Instances/VMs mount block storage, which are virtual hard disks. EC2 has EBS for that. 

S3 is great for large scale data storage, distribution, or upload. Great for offloading as well.

Shrink instance data usage onto S3 as well

S3 should be your default thought for any input to AWS or output from AWS. Most services which consume data and or output data can have S3 as an option to take data from or put data to when its finished.

Exam questions: S3 should be default if there's a number of answers of where to put data. 

---

# Week 2 Day 8

## Cloud Formation Basics

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


## How does CloudFormation use templates

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

# Week 2 - Day 10 (SKIPPED DAY 9 CAUSE IM BABY)

## CloudWatch

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
## Shared Responsibility Model
Shared Responsibility Model: how AWS provide clarity around which areas of systems security are theirs, and which are owned by the customer.

AWS responsible for security OF the cloud
Customer is responsible for security IN the cloud.

AWS is responsible for managing the security of the AWS regions, AZs, edge locations, the hardware and security of the global infrastructure. Same with Compute, Storage, Database and Networking services AWS provides us. Any software which assists in those services as well is managed.

Customer responsibility:
Client side data encryption, integrity and authentication, serverside encryption (file system and/or data), networking traffic protection (encryption, integrity, identity)
OS, network, firewall configuration, applications, paltform, identity and access management, customer data.

---
## High Availability vs Fault Tolerance vs Disaster Recovery
High Availability: HA
Fault Tolerance: FT
Disaster Recovery: DR

### HA:
Aims to ensure an agreed level of operational performance, usually uptime, for a higher than normal period.
HA isn't aiming to stop failure. outages are still probable.
An HA system is designed to be online and provide services as often as possible. If fails, its components can be replaced or fixed as quickly as possible often using automation.

Not about user experience. If something fails and disrupts usage for a few seconds, thats okay. Still HA. Maximizes systems online time.

System availability is expressed of percentage of uptime:
99.9% (three 9s) = 8.77 hours of downtime per year
99.999% (five 9s)= 5.26 minutes of downtime

### FT:
similar to HA, but more.
Property that enables a system to continue operating properly in the event of the failure of some (one or more faults within) of its components.

If a system has faults, it should continue to operate properly while its being fixed, without impacting customers.

FT = operate through failure. can be expensive since it is complex to implement.

### DR:
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

---
# Day 11 (3/14)

## DNS Refresh
DNS REFRESH

All layesrs in DNS hierarchy are zones.

Path and hierarchy with example:
Machine/person wants to go to www.netflix.com, but needs the IP address for it.
www.Netflix.com is the DNS name.
Final point: a dns zone for netflix.com that has the answer that I need, a record that links www.netflix.com to the IP address.

DNS helps navigate this process through steps/zones.

Local machine queries for netflix.com

1. First step: check local DNS cache and hosts file of the machine. Hosts file: static mapping of DNS names to IPs, and overrides DNS.
If the local cache doesn't have the DNS name, next step:
2. DNS Resolver: Sends to resolver, a DNS server running on a router or within an ISP. Does the query on our behalf. Query is received by resolver and takes it from here. It also has a local cache it checks first. If it finds it in the cache, may return a non-authoritative answer. If no cached entry, next step:
3. Resolver queries the Root Zone via one of the root servers. Root zone isn't aware of www.netflix.com, but helps us get closer. root zone contains nameserver records which point at the nameservers for the .com TLD. The root zone returns the details of the .com Name Servers to the Resolver. Not what we're looking for, but one step closer. Resolver now has the records for the .com Name Servers. Next step:
4. Resolver queries the .com TLD name server for www.netflix.com. www.Netflix.com is registered in .com, .com zone contains entries for netflix.com. The .com name servers don't have the answer for the resolver, one step closer. The details of the netflix.com name servers from the .com TLD name server are returned to the resolver. Next step:
5. Resolver queries the netflix.com name servers for www.netflix.com. These are the final stop, they host the zone and zone file for this domain, and are being pointed at by the .com TLD zone, the result will be authoritative answer back to the resolver. The resolver now caches the result, then returns it to the main machine.

AI Cleaned up version:

Step 0 — User request
User enters: www.netflix.com

Step 1 — Local lookup
The machine checks:
hosts file
local DNS cache

If not found →

Step 2 — Recursive resolver

Query goes to:

ISP DNS
home router DNS
8.8.8.8
1.1.1.1

Resolver checks its cache.

If not found →

Step 3 — Root servers

Resolver asks a root server:

Where is www.netflix.com?

Root response:

I don't know www.netflix.com
but ask the .com nameservers

Root returns:

NS records for .com
Step 4 — TLD servers

Resolver asks the .com servers:

Where is www.netflix.com?

TLD response:

Ask the nameservers for netflix.com

Returns:

NS records for netflix.com
Step 5 — Authoritative nameserver

Resolver asks the netflix.com authoritative servers:

Where is www.netflix.com?

Authoritative response:

www.netflix.com → 54.x.x.x

This is an authoritative answer.

Step 6 — Response

Resolver:

caches result
returns IP to client

Client connects to the server.

## Route 53 Fundamentals

Route 53: service in AWS that allows you to register domains. It can host zone files on managed nameservers it provides.
Global service with a single database. Replicated between regions, globally resilient. Can tolerate failure of one or more regions with no issues.

Registering Domains:
R53 has relationships wil all of the major domain registries, which manage the TLDs (.com, .io, .net, .org).

When a domain is registered:
1. R53 checks with the registry for the .org TLD to see if the domain is available. 
2. R53 then creates a Zone File for the Domain being registered.
--Zone file is a database that contains all of the DNS info
3. R53 allocates, creates, and manages name servers for this zone. Distributed globally. Usually 4 NS per zone. Zone file = Hosted Zone in R53 terminology. Puts zone file in all 4 NS/
4. Communicates with the .org registry and adds the name server records into the zone file for the .org TLD by using NS records. In other words, adds NS records to the .org zone, which indicates that our four NS are all authoritative for the domain.

## Zones in R53
R53 provides DNS zones as well as hosting for those zones. 
DNS as a service. Lets you create and manage zone files in AWS, which are called Hosted Zones in R53 terminology, because they are hosted on AWS managed name servers.
When a hosted zone is created, a number of servers are allocated and linked to that hosted zone. From R53 perspective, every hosted zone also has a number of allocated managed name servers. 
Hosted zone can be public or can be private, linked to VPCs.
Hosted zone hosts DNS records
Inside R53, you'll see record sets. 

## Demo
Straight forward

## DNS Record Types

### Nameserver Records/NS Records:
Allow delegation to occur in DNS.

Example: .com zone will have multiple NS records inside it for amazon.com. This is how the delegation happens.  They point at servers managed by the amazon.com team and the servers host the amazon.com zone. Inside this second zone, there are DNS records as well, such as www. Same on other side using the root zone. Root zone has delegated management of .com by having NS in the root zone point at the servers that host the .com zone.

How the root zone delegates control of the .x to the .x registry

### A records/AAAA records

Both are the same.

DNS Zone: google.com
These map host names to IP addresses. Difference is the type of IP address.

WWW host = an A record maps this onto an IP version 4 address.
AAAA recprd type is the same, but maps the host into an ipv6 address.

Normally create both. Client OS/DNS software will pick the one it wants to use.

### CNAME
CNAME: Canonical name.

For a given zone, the CNAME record type lets you create the equivalent of DNS shortcuts, so host to host records.

Example: ARecord called Server -> points to IPv4
Server usually performs multiple tasks (ftp, mail, www).
You can make a CNAME for all of these and have it point to the ARecord, means they will all resolve to the same IPv4.
CNAME reduces admin overhead. If the IPv4 changes, it's just the single Arecord to update.
CNAMES cannot point directly at an IP, only other names.

### MX Record Type

MX Records have two main parts: priority and value
the value can be just a host, such as mail. since its inside a zone, then it would be the mail.zone. for example, google.com zone, MX record with mail as value = mail.google.com 
If you include a . to the right, its a fully qualified domain name. so this can point at something outside the zone.
same example: if we're sending an email to hi@google.com, our email server looks at this email (to:), focuses on the domain, so google.com
Then does an MX query using DNS on google.com. Same DNS process, so after going through root, dot com, then google.com, then retrieves MX records. Here, it uses the Priority value for the MX record to know which to use. Lower number is higher priority, or same values mean any will work. Second priority will be used if first isn't functioning. Server returns highest priority back and uses this to connect to the mail server via SMTP to deliver mail.

MX Record: how a server can find the mail server for a specific domain. Server sending the email uses an DNS to do an MX lookup and locate the mail server to use.

### TXT Record

TXT records allow you to add arbitrary text to a domain. Allows DNS to provide additional functionality.

Common usage: prove domain ownership. Adding a mail server like google, they will ask if you own the domain. they'll give you a text phrase to add to a txt record. you add the txt record to your zone, then google queries for it, sees it, confirms ownership.

Can also be used to fight spam: add certain info to a domain indicating which entities are authorized to send email on your behalf. If any email servers receive emails from any other servers, then it is spam.

### DNS TTL

TTL: Time to Live. TTL value can be set on DNS records, numeric value in seconds. It is used for cached information. When a DNS query goes through, it eventually reaches the nameserver it needs to get an authoritative answer. That answer is then cached in the resolver server that did the DNS query for you, which is usually hosted at the ISP. if someone else with the same ISP/resolver performs the same DNS query, it'll use the cached result at the resolver since its quicker. But since this is cached, if theres an update on the NS zone, then it could delay correct info being updated since the cached info will be wrong now. TTL solves that, once it hits that time, it'll remove and update with the next query.

If you need to change DNS records, recommended to lower TTL value well in advance of the work to avoid caching issues.

## Quiz Info
Registry maintains zones for a TLD
Registrar has relationships with the .org TLD zone manager allowing domain registration.
Default VPC has subnets equal to the number of AZs in the region the VPC is located in.
IP CIDR of a default VPC: 172.31.0.0/16
AMI Permission Options: Public access, owner only, specific AWS accounts
Not stored in AMI: instance settings, network settings
EC2 is IAAS
AWS Public Service: located in the aws public zone, anyone can connect, but requires permissions to do so
AWS private service: located in vpc, acccessible from vpc it is located in. Also from other vpcs or on premises networks as long as private networking is configured,
S3 is an aws public service. object storage system. buckets can store unlimited amount of data.
CloudFormation Logical Resource: resource defined in a cloudformation tempalte
Cloudformation physical resource: a physical resource created by creating a cloudformation stack.
HA: system which maximizes uptime
FT: system which allows failure and can continue operating without disruption
13 DNS root servers exist
12 large orgs manage DNS root servers
DNS root zone managed by IANA
A Record: converst HOST into IPv4
NS Record: how root zone delegates control of .XXX to the .XXX registry

---
# Day 12 - Review!

## DNS/Route53 Review

Explain this chain:
Client
→ Recursive Resolver
→ Root Zone
→ TLD Zone (.com)
→ Authoritative Zone (example.com)
→ DNS Record → IP

Client types in a web address, lets use www.google.com as an example.
First, local device checks cache for result. If none, then it sends query to resolver, usually ISP or router. It checks its cache, if nothing, resolver queries the root zone for www.google.com. root zone says not sure, but ask the .com nameservers. returns Ns records for .com back to resolver. resolver now queries .com TLD for www.google.com. TLD says not sure, but ask the nameservers for google.com. returns NS records for google.com to resolver. resolver then queries the google.com authoritative nameserver for www.google.com, which it does know. authoritative nameserver has IP address for www.google.com, returns it to resolver. resolver then sends back to original device with query. this would be an authoritative answer since it got it from the NS, which is an authoritative zone. if the answer was obtained from cached information in either local machine or resolver, it would be nonauthoritative.

Review these terms:
DNS zone: a section of the DNS namespace managed by an authority, such as google.com zone. Inside are DNS records: google.com, mail.google.com, ftp.google.com
Zone file: the file that stores the zone on the disk (had to look it up)
Authoritative Answer: one true answer when doing a DNS query/an answer returned from an authoritative dns server for the requested zone. for example, getting the IP address of a url from the nameserver through DNS search.
Non-authoritative Answer: when a DNS query is resolved through cached local files, either in resolver or local machine cache files. a
TTL: time to live. time it takes to remove something. example: DNS TTL. a cached non-authoritative answer will have a TTL to make sure if there are any updates to the original DNS records, the cache file does not have the incorrect info indefinitely. how long a dns record may be cached before it must be refreshed from the authoritative server.
Nameserver: dns server which hosts one or more dns zones, and stores 1 or more zone files. a DNS server that hosts zones and answeres DNS queries.

Ok on the Record Types as I just went through them. Cantrill did not mention ALIAS, can you explain that?

For the concept check on Registrar/Registries, which cantrill course do I review? the tech fundamentals about registering a domain? and r53?