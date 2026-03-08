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


---
Day 5 (3/8)

Public vs Private AWS services

Public vs Private is referring to networking only. 

Public AWS service is something that is accessed using public end points, like S3.

Prviate runs within a VPC.

Permissions still exist.

Public Internet Zone ------ Private Network

Private network is home network.
AWS has private zones, which are called Virtual Private Clouds, or VPCs. Isolated, so can only communicate with each other if you allow it, same with internet. 

Services can be placed inside VPCs, like EC2.

Third zone, AWS Public Zone. Runs between public internet and private zone/VPCs. A network connected to the public internet.

AWS Global INfrastructure

AWS has created their infrastructure platform to be a collection of individual infrastructure located worldwide. 

2 types of deployments at this level:
AWS regions - doesn't directly map onto a continent or a country. Region is creation of AWS. Inside this region is a full deployment of AWS infrastructure. Full compute, storage, DB, AI, analytics
They add regions all the time. 

Some countries have one region, some have multiple. geographically spread. 

AWS Edge locations: much smaller than regions, only have content distribution services and some type of edge computing. are located in many more places than regions. Netflix uses edge locations to store content as close to customers as possible. local distribution points. 

Netflix can run infrastructure from multiple regions, but content will be stored in edge locations.

Some services are global, not region specific. IAM is global, EC2 is region.

Regions have 3 benefits:
1. each region is separate geographically. attack or disaster in one region wouldn't affect another region. regions are designed to be 100% isolated, achieves fault tolerance.

2. select a region, you have geopolitical/governance separation. 

3. location control, allows you to tune your architecture for performance. duplicate infrastructure into another region if demand is there.

Whats inside regions

Referring to regions:
Region code, known as the region
or the region name

One region in AUS, code is ap-southeast-2, name is Asia Pacific (Sydney)

Availability Zone, AZ

Inside every region, aws provide multiiple AZs. Sydney has 3:
ap-southeast-2a
ap-southeast-2b
ap-southeast-2c

AZs are isolated infrastruction inside a region. if a region experiences an issue, it can be in only one AZ, other two AZs would be okay.

AZs are not data centers. They can be part of multiple data centers. They are mainly just isolated from other AZs.

Can place services in multiple AZs for resilience. 

how to define resilience of aws service

Globally Resilient: relatively few of these in AWS. Service operates globally with a single database, one product, and its data is replicated across multiple regions inside AWS. A region can fail, but the service would continue running. Full outage = full world failure. IAM and Route 53 are global.

Region Resilient: operate in a single region with one set of data per region. data replicated within AZs in a region, so if AZ fails, another will back up. but if region fails, service will fail. 

AZ resilient services: run from a single AZ. If the AZ fails, the service will fail. 


---

VPC (Virtual Private Cloud) Basics

VPC is a service you use to create private networks inside AWS that other private services will run from. VPCs are also the service which is used to connect your aws private networks to your on premises networks when creating a hybrid environment. also lets you connect to other cloud platforms when creating a multi cloud deployment. 

A VPC is a virtual network inside of AWS. A VPC is within 1 account and 1 region. They are regionally resilient. Operate between multiple AZs within a region.

A VPC by default is private and isolated. Services deployed within the same VPC can communicate, but isolated from other VPCs and public AWS zone and public internet, unless you configure it otherwise.

Default VPC and Custom VPC

Default VPC - 1 per region per aws account
Custom VPCs - many per region per aws account

deafult vpcs are created by aws and there is one per region created by default. preconfigured specficially and networking handled by AWS. less flexible than custom vpcs.

VPCs are created within an AWS account -> region. us-east-1 region example

VPCs cannot communicate across themselves unless you allow it. entirely private by default.

us-east-2
default vpc

every vpc is allocated a range of ip addresses, VPC CIDR. CIDR defines start and end range of ips vpc can use. everything inside vpc uses cidr range. if you allow communication, it needs to communicate through cidr, same outgoing.

custom vpcs can have multiple cidr rangers, but default only gets one. always the same:
172.31.0.0/16

VPC provides resilience: can be subdivided into subnets. each subnet is located inside one AZ. set on creation, cannot be changed.

default VPC: always configured the same way: one subnet in every AZ in that region. us-east-2 has 2a, 2b, and 2c. so a subnet in each of them. if that az zone fails, the subnet also fails. but other subnets in other azs will be good.

default vpc:

one per region, max. can be removed and recreated.

some services withihn aws assume default vpc will be present, so leave them active, but can be left alone.

default vpc cidr is always 172.31.0.0/16.

inside every default vpc, a smaller /20 subnet in each az is created in the region. varies depending on az number.

default vpc comes with a few things preconfigured:
internet gateway (igw): allows vpc to connect with internet
security group (sg) and NACL -> security features to limit incoming/outgoing data transfer

by default, anything placed in the default vpc subnet is assigned a public ip address.subnets are configured to provide anytyhing that is deployed into those subnets with public ip addresses. specific to default vpc only. any services will have a public ip address.

---
Elastic Compute Cloud (EC2) Basics

deafult compute service within aws

EC2 provides access to VMs known as instances

EC2 is IaaS. provides access to VMs known as EC2 instances. unit of consumption is instances, which is just an OS configured in a certain way.

private service by default, runs in private aws zone. uses VPC networking. configured to launch into a single vpc subnet. you set it when you launch the instance. you need to configure public access if needed. vpc needs to support public access though.

EC2 is az resilient. if the az fails, the instance will fail too. 

instances can have different sizes and capabilities. some can be changed after launching.

consumer manages OS and upwards on the stack. offers on demand biling. by the second or hour depending on the software.

instances can use a number of differnt types of storage. two popular: storage that is on local hostr, or elastic block store (EBS). network storage made available to the instance.

instances have a state, which is a status:
running
stopped
terminated
others also exist, but these for now.

launch an instance -> running state
instance shut down -> stopped
instance restart -> running

kind of like switching off an appliance.

termination -> one way change. once you do it, non reversible. pretty much deleted.

AMI -> amazon machine image -> in an image of an ec2 instance

ami can be used to create an ec2 instance or create ami from ec2 instance. preset.

ami contains attached permissions, control which accounts can and cant use ami.

ami can be set to public, everyone allowed, anyone can launch instances from that ami.

owner of an ami is implicitly allowed to create ec2 instance from that AMI. 

add explicit permissions to ami where the owner explicitly grants access to that ami for specific aws accounts. private or add other aws accounts or public.

ami handles 2 more things:
contains the boot/root volume of the instance. C: in windows. drive that boots OS. can contain other volumes too, but at least one, boot volume.

Block device mapping: configuration which links the volumnes that the ami has and how they are presented to the OS. determines which volume is the boot volume, and which is the data volume. os expects to receive volumes and device id. block device mapping links volume to device id.

ec2 instances can run different os. distro of linux, different versions of windows. windows connects with RDP (remote desktop protocol, port: 3389).

linux uses SSH protocol, port 22. when connecting to linux, you login using SSH key pair. 

from cmd or linux shell or putty, you connect to ec2 instance. when you create ec2 instance, you pick a key pair to use. either create or use an old one. when you create, it makes two parts of the same key. private key: download once and only once. public, aws keeps, and when you select to use that key pair, public key is placed on an instance. private key is how you authenticate to the instance since they match. slightly different on whether its linux or windows. linux uses private/public key to authenticate and connect to the instance. windows you need to provide the private key to gain access to the lcoal admin password of the instance, then use RDP using local admin user and password.


