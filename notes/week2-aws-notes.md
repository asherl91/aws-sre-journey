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