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

# Day 14

Week 1 and 2 Review

## Week 1

### OSI Layers
Layer 1 - Physical -> bits (cables, signals)
Moves bits physically via electricity/light. No addressing, no logic.

Layer 2 - Data Link -> frames, MAC (local delivery)
Delivers data within a local network. Uses MAC addresses + frames

Layer 3 - Network -> IP, routing (between networks)
Moves data between networks. Uses IP addresses. Routers operate here.

Layer 4 - Transport -> TCP/UDP, ports (applications)
Handles communication between applications. Uses ports. Provides reliability (TCP) or speed (UDP)

(dont worry too much about these)
Layer 5 - Session -> sessions
Layer 6 - Presentation -> formatting/encryption
Layer 7 - Application -> user level apps (HTTP, etc)

Frames are layer 2, MAC
Packet is layer 3, IP
Segment is layer 4, TCP/UDP data

Encapsulation:
Segment -> Packet -> Frame -> Bits

### ARP
Finds MAC address from IP.

Process: Broadcast into all Fs = Who has IP?
-> device replies with MAC

### Subnetting
BITS!!!!!!!

192.168.1.10/24
192.168.1 = network
10         = host

/## equals the amount of network bits in the ip address. everything else is hosts

I believe hosts are machines on network, then networks are well all the ips.

subnet mask defines which is network and which is host. this is just the representation of /## in bits. so /24 would be 11111111.11111111.11111111.00000000
1 for network, 0 for host

formula:
hosts = 2^(host bits) - 2
got it.

im okay at converting binary and understand that math, just need help here identifying.

example:
/24:
32 - 24 = 8 host bits
2^8 = 256
256 - 2 = 254 usable hosts

Same subnet:
means same network
uses subnet mask to determine

192.168.1.10
192.168.1.20
/24

192.168.1 == same

why use ARP
applications still use IP, but the network delivers using MAC

So flow is
I want to send to 192.168.1.x from 192.168.1.x
->needs MAC to send the frame, so ARP is used.

IP is who, target.
MAC = how its delivered.

different subnet:
device sends to router. 
checks subnet mask for local availability
192.168.1/24 is not 192.168.2/24, different networks.
not local, send to router. 

subnet check is done at device, OS level.

how does it determine this, with ARP still? (default gateway = 192.168.1.1 on router. it cant find the ip, so it sends it to default router to investigate)
router forwards to next network

### Router
Router:
1. receives frame
2. removes frame (opens it up to see inside)
3. looks at ip packet
4. decides next hop -> more info here please
5. builds new frame
6. sends to destination

### Packet Flow
ok so this is just layer 4 -> 1 for networking stuff
Application -> HTTP request (technically l7?)
Transport -> TCP (port 443) (so l4)
Network -> IP Packet (l3)
Data link -> Frame with mac (l2)
Physical -> Bits sent (l1)
Then it goes opposite, correct?

App -> TCD/UDP -> IP -> Frame -> Bits

Frame changes because MAC = local delivery only. but all devices have mac, so it needs to keep finding it to hop. so laptop -> router -> server:
source mac = laptop
destination mac = router
router receives frame, removes frame, looks at IP, decides where to go.
router creates new frame, source mac is router, destination mac is next device, which it gets from ARP by using the IP. so mac is used on other layers than just l2.

IP stayes the same due to end to end identity, source -> destination. I think i get this one.

### NAT 

NAT assigns ports in a local private network with multiple devices so they can use one public IP. but the port assigned locally/privately is not the same as the public port.

NAT Table, i remember this. kind of a cache? explain this a bit more on why its needed and how it works.



### EBS
Block storage, acts like an HDD, attaced to one EC2 instance. (AMI does create a snapshot though)
low latency, persistent, used for os/apps

Should I know what block storage means exactly? I get its what its what EBS uses and acts like a hard drive, but not sure what block storage means. is that just the storage format of regular hard drives? 

### S3
object storage, accessed via API, not tied to a machine. object storage means its just storing everything at the same level, but there are ways to organize with prefixes.
massive scale, high durability, used for files/backups/data.

EBS = disk, S3 = file storage system

## Week 2

### EC2
Launch components, must configure:
AMI (OS)
Instance Type (t3.micro)
Key Pair
Network (VPC/Subnet)
Security Group
Storage (EBS)

AMI includes:
OS
Filesystem/data
Installed software
Config
Snapshot of EBS

Does not include: running state (RAM, processes), network, security groups, multiple resources, architecture

Security Group:
A virtual firewall attached to an instance. Controls inbound/outbound traffic
security group (sg)  -> security features to limit incoming/outgoing data transfer

STATEFUL: if inbound is allowed, response traffic is automatically allowed

From my notes, all I have:
"Stateless firewall: needs to check each side in a transmission, so needs rules for outbound and inbound

Stateful firewall: understands communication is both ways, checks both with less rules"

Stateful firewall tracks connections. Stateless requires separate rules for outbound and inbound. But Stateful recognizes a connection and only requires one rule. Stateful better, fewer rules needed,  easier to manage, less error prone.

Why use stateless?

Key pair is for SSH connection. No recovery if lost private key.

### S3

Bucket = container for objects
Object = file (data + metadata)
Key = full path/name of object inside bucket

11 nines = durability
99.999999999% durability = extremely unlikely to lose data. does not have to do with uptime.

Global uniqueness:
Bucket names exist in a global namespace -> must be unique across ALL AWS accounts

S3 is a global service (api endpoint), but buckets are created in a specific region and data lives there. 

Public bucket risk:
Data exposure to the entire internet -> biggest AWS security risk

### Cloudformation

Cloudformation automates infrastructure creation. Replaces manual clicking with repeatable templates.

Template:
Template = blueprint in JSON/YAML
Stack = running instance of template
Resources = actual AWS services created

IaC importance (I dont think Cantrill went over this actually)
Consistency, Repeatability, Automation, Version Control. 
This just helsp with errors on reapeating manual clicks. Instead of creating EC2 instances over and over and accidentally messing up, create one CF template and reduce errors.

### CLoudWatch

Cloudwatch provides Metrics, logs, and alarms.
Basic monitoring: 5 minute intervals, free
Detailed = 1 minute intervals, paid

### Core Concepts

Shared Responsibility
AWS =  security OF the cloud
You = security IN the cloud

HA/FT/DR
HA = minimize downtime
FT  zero downtime
DR = recovery after total failure

### DNS/Route 53

DNS Zone
A database of DNS records for a domain. 
A zone = a portion of the DNS namespace you control.
Example.com, the zone for example.com contains records like:
www.example.com -> IP
mail.example.com -> mail server
api.example.com -> IP

Zone = container of records for a domain

Does not have to include everything under the domain.

In AWS (Route53)
Hosted Zone = AWS version of DNS zone
Inside:
A Records
CNAME
MX
TXT

Nameserver:
A DNS server  (software) that hosts zones. Runs on physical/virtual infrastructure. 
Nameserver answers DNS queries using zone data.
Nameserver = the server that hosts the data
Zone = the DATA

Zone: example.com
Stored on: ns-123.awsdns-45.com -> Nameserver

TTL expires:
Cache entry expires -> resolver must fetch fresh data

Registry = manages TLD (.com)
Registrar = sells domains to users

Record types:
A = domain -> IPv4
CNAME = domain -> another domain
MX = email routing
TXT = text/verification data

ALIAS: AWS sepcific record. Like A record but can point to AWS resources (ELB, Cloudfront). Used because CNAME has limitations at root domain.

### Extra: AMI vs CloudFormation

AMI (Amazon Machine Image)
A snapshot/template of a SINGLE machine.

Defines:
OS
Installed software
filesystem
configuration
EBS snapshot

Creates one EC2 instance

Does not include: running state (RAM, processes), network, security groups, multiple resources, architecture

AMI = prebuilt PC image

Cloudformation:

A blueprint for ENTIRE infrastructure

Can create:
EC2 instances
VPCs
subnets
security groups
S3 buckets
load balancers
EVERYTHING

CF = blueprint for entire house
AMI = one appliance inside

### Extra: VPC

VPC = virtual provate cloud, your own isolated network inside AWS
Inside vpc, control IP, who can talk to what, what is public vs private

VPC contains:
subnets
route tables
internet gateway
security groups
resources

CIDR Block (IP Range)

When you create a VPC, you choose
10.0.0.0/16
which means
All resources in this VPC get IPs from this range

Subnets:
You split your VPC into smaller chunks with subnet -> /24
Each subnet exists in one AZ, groups resources together

Public subnet has route to internet gateway, private does not.

Internet Gateway IGW
Allows communication between VPC and internet.

Route Tables:
If traffic is going here, send it there

Default VPC:
AWS gives you 1 default vpc per region. includes public subnets, internet gateway, basic routing.

VPC = networking foundation of aws.