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