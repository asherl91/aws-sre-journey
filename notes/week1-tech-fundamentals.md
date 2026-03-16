# Day 1 (3/3)

## OSI Model Intro:

OSI (Open Systems Interconnection) model is a framework that describes the functions and interactions of computer systems in a network. It is divided into seven layers, withe ach layer responsible for specific funcstions in data transmission.

Local Networking - Ethernet
Routing -> Goes from local to online

Networking stack:
Host Layers:
Layer 7: Application
Layer 6: Presentation
Layer 5: Session
Layer 4: Transport
---
Media Layers:
Layer 3: Network
Layer 2: Data Link
Layer 1: Physical

---

## Layer 1: Physical:

For example, LAN, connecting two PCs via cable through network cards.
Physical mediums:
Copper: electrical
Fibre: light
Wifi: Radio Frequencies (RF)

Layer 1 specifications define the transmission and reception of RAW BIT STREAMS between a device and a SHARED physical medium. It defines things like voltage levels, timing, rates, distances, modulation and connectors.

Layer X device = Understands Layer X and below. So Layer 3 can use Layer 1-3, but layer 1 device only communicate with layer 1.

Layer 1 device: Network hub. multiple ports, all devices receive same data. No device addressing. NOT A SWITCH.

Collision: two or more devices transmitting at the same time. Renders info useless.

L1 has no media acess control and no collision detection

---

## Layer 2: Data Link

Layer 2 network requires a fuinctional layer 1 network to operate. Hihgher layers build on lower layers adding features and capabilities.

Frames are a format for sending information over a layer 2 network. 

### MAC
MAC Address: unique hardware address for every device on a network. Hexadecimal address, 48 bits long in hex, 24 bits for manufacturer. Frames can be addressed to a destination or broadcast (ALL F'S)

Mac address not software assigned.

MAC address 2 parts: OUI, Organizational Unique Identifier. assigned to companies who manufacture netwwork devices. 
Network Interface Controller, NIC specific.
MAC addresses shoudl be globally unique.

Layer 2 provides frames, while layer 1 handles the physical transmission and reception onto and from the physical shared medium. Layer 1 doesn't understand the frame, just raw data.

Frame = container. 

Parts of frame:
Preamble: Allows devices to know the beginning of the frame

//Mac header:
Destination Mac Address
Source Mac Address
ET (EtherType), l3 protocol used for frame//

Payload, actual data. Data the frame carries from source to destination. Its generally provided by layer 3, and the ET attribute defintes which l3 protocl is  used
FCS: frame checker

CSMA: carrier sense multiple access. It checks to see if there's a carrier before sending. Carrier means something already being sent, so if PC 1 is sending something to PC 2, but PC2 wants to send to PC 1 at the same time, carrier is in effect for PC1, and PC2 waits until its finished. Avoids collision.

Encapsulation: Process of taking some data and wrapping it in something else. Game data -> frame. ONce sent, its de-encapsulated.

CD: collision detection. can happen if both sides send at the same exact time and CSMA doesn't detect carrier. If collision detected, jam signal is sent by all devices and a random backoff occurs. 

Backoff: period of time where no device sends transmission. its random, so one device will try first.

Switch: level 2 device. helps with collisions.
Switches understand frames and Mac addresses. They maintain a mac address table with starts off empty. As the switch receives frames on its prots, it learns which devices are connected and populates the mac address table.
Switches store and forward. They verify during storage and then send to appropriate location. Due to this, avoids collisions.

-----

# Day 2 (3/4)

## Layer 3: Network/IP

Layer 3 gets data from one location to another.

Remember, each hierarchal layer requires a lower layer to work, so layer 3 requires one or more operational layer 2s

Ethernet is a l2 protocol used generally for local networks. Long distance point to point link swill use other more suitable protocols, such as PPP/MPLS/ATM.

Inter-networking: move data between different local networks. Need layer 3 for this.

Layer 3: common protocol which can spam multiple different layer 2 networks. 

Layer 3 adds IP (internet protocol), which then adds IP Addresses, which helps identify devices.

IP is a layer 3 protocol which adds cross network IP addressing and routing to move data between local area networks without direct P2P links. 

IP packets are moved step by step from source to destination via intermediate networks. Encapsulated in different frames along the way. 

Routers, layer 3 device, move packets of data across different networks. Remove frame encapsulation and add new frame encapsulation at every hop.

IP packets similar to frames, just level 3. Frames are generally local, while ip packet can be fully remote. 

IP packet moves across layer 2 networks, placed inside frames, encapsulation

Two version of IP
v4 - classic
v6 - new

IP packets: have more stuff than frames, but some aren't needed right now.
v4:
All packets have source IP address and destination IP address. Source ip address is the device IP which generates the packet, and the destination ip address is the intended destination ip for the packet. 

Protocol field: while IP is a protocol, this protocol comes from layer 4. examples are icmp, tcp, udp.

TCP data = protocol 6
PINGS (ICMP) = protocol 1
UDP = protocol 17

Data is the majority of the packet. 

TTL = time to live, defines how many hops the packet can move through. used to stop packets from looping around forever.

v6:
Source and Destination IP addresses, similar but way bigger. 

Data from layer 4 protocol. 

TTL = hop limit in v6. similar idea.

---

## IP Addressing (v4)

133.33.3.7 = format is Dotted Decimal Notation
Four decimal numbers from 0-255, separated by dots. 

All IP addresses have two parts

Network part are the first two numbers, represents which IP network the IP address belongs to
Host part is 3rd and 4th numbers, represents hosts on that network.

IP addresses are binary, each number is 8 bit, so total 32 bits. 8 bits = octet

/16 prefix = first 16 bits of the IP are the network, and the remaining bits are for hosts. 

Static IPs, set by humans, won't change. Assigned automatically by machines (DHCP, Dynamic Host Configuration Protocol).

IPs need to be unique on both a network and globally

Subnet masks, configured on layer 3 interfaces along with ip addresses.

Default gateway. IP address on the local network which packets are forwarded to generally if the intended destionation is not a local ip address.

Subnet masks allows an IP device to know whether the IP address its communicating with is on the same network or not. Influences if the device attemps to communicate directly on the local network or if it needs to use the default gateway. 

Subnet mask: allows a HOST to determine if an IP address it needs to communicate with is local or remote. Influences if it needs to use a gateway or can communicate locally. 

Router is usually default gateway in local networks. 

Route table has list of where to send packet, depending on destination IP. Matches closest one. 

ARP, Address Resolution Protocol. Used when you have l3 packet and want to encapsulate it inside a frame and then send to MAC address. Dont know the MAC address though, so need a protocol that can find the MAC address from a given IP address. ARP will do this.

ARP broadcasts on layer 2. ARP frame of all fs (broadcast) to determine who has the IP its looking for. 

Summary:
IP Addresses, v4/v6, cross network addressing
ARP -> finds mac address for IPs
Route: where to forward packet
Route tables: multiple routes it can take
Router: moves packets from source to destination, encapsulating in l2 on the way
device <-> device communications over the internet
No method for channels of communications, just knows send data from src ip to dst ip
Can possibly be delivered out of order due to the above. 

---

## Layer 4, transport layer Kind of Layer 5 too

Layer 4 and 5 are kind of similar, OSI is conceptual

Layer 4 adds TCP and UDP

TCP: Tranmission Control Protocol
UDP: User Datagram Protocol

Both run on top of IP. 

TCP when you want reliability, error correction and ordering of data. Slower/more reliable. Used for most important application lab protocols, such as HTTP, HTTPS, SSH, etc.

TCP is a connection oriented protocol, means you need to set up a connection between two devices, once setup, creates bidirectional channel of communications.

UDP is faster, less reliable. Doesn't have the TCP overhead required. All performance, lowers reliability. 

Focusing on TCP.

TCP introduces segments, another version of frames or packets. Segments are specific to TCP. TCP segments are placed inside packets. Segments dont have source/destination IP addresses, because it uses the packets src/dst.

Segment parts:
Source and Destination Ports:
POrts help have multiple communications per device. 

Sequence: a way of uniquely identifying a particular segment within a particular connection to make observations.

Acknowledgements: indicates if a sequence number has been received. 

Flags n things: allows various controls over TCP. can be used to close connections or sync sequence numbers, data offset, etc. Not very important right now.

TCP window: defines the number of bytes that you indicate you are willing to receive between acknolwedgements. Once reached, sender will pause until you acknowledge that amount of data. Flow control. 

Checksum: used for error checking. Can arrange for retransmission if error found.

Urgent pointer: ???

TCP is used to allow communication between two devices. Connection based. Provides a connection architecture between two devices. Client and Server. Connection is established using a random port on a client and a known port on the server. Once established, the connection is bidirectional. The connection is a reliable connection, provided via the segments encapsulated in IP packets. 

Flags n Things: Flags which can be set to alter the connection:
FIN can be used to close (finish)
ACK for ackonwledgements
SYN to syncrhonize sequence numbers

Before using TCP, a connection needs to be established. 3 step handshake
Step 1 SYN: client needs to send a segment to the server. Segment contains a random sequence number from client to the server, unique to this direction of travel. Sequence number is initially set to random value known as the ISN, initial sequence number CS. Client saying to the server Lets talk, then sets sequence number.

Step 2 SYN-ACK: Server receives segment and needs to respond. Picks its own random sequence number SS. Acknowledge that its received all communication from the client. So it takes client sequence and adds 1. Sets the acknolwedgement part of the segment that its going to send to CS + 1 value. Essentially letting the client know its received previous transmission, and wants client to send next part of data, sending it back. Sure, lets talk. SYN ACK, used to synchronize squence numbers and also acknowledges receipt of client sequence number. 

Step 3 ACK: Client receives segment from server. KNows server sequence. Acknowledges to the server, takes SS adds 1. SS+1 = acknowledgement value. Also increments its own CS to CS+1, sets CS+1 to sequence. Sends acknowledgemnt segment containing everything back to server, saying lets go.

Connection now established. Anytime either side sends data, they increment the sequence, then other side acknowledges the sequence value + 1. This allows for retransmission in case data is lost. 

Stateless firewall: needs to check each side in a transmission, so needs rules for outbound and inbound

Stateful firewall: understands communication is both ways, checks both with less rules.

---

## NAT

NAT: Network Address Translation

NAT is a process designed to address the growing shortage of IP v4.

IP v4 addresses are either publically routable or fall in the private space. Public IPs are given to ISPs to then assign to consumers. They have to be unique. 

Private addresses, 10.0.0.0 range, can be used in multiple places, but can't be routed over the internet. To give internet access to private devices, we need NAT.

NAT provides additional security benefits.

Multiple types of NAT. They all translate private IP into public, then reverse translates back for response communication. 

Static NAT - 1 private to 1 (fixed) public address (IGW)
Where you have a network of private IP v4 aDDRESSES and can allocate a public ip v4 address to individual private ips. So the static nat device translates from one specific private address to one specific public address. Giving that private address access to the public internet in both directions.

Static NAT is what you would use when certain specific private IPs need access to the internet using a public IP and where these IPs need to be consistent. 

Devices on private network cannot access CatAPI, a public service, since they are private IPs and private only. Router (NAT DEVICE) is between both private home network and public internet network. Router maintains a NAT table, maps PrivateIP : PublicIP (1:1 device mapping). Any private device will have a dedicated, allocated public IP.

Private device wont have the public IP configured on it, its just an allocation. Packet is created with private ip as source. Passes through router, NAT table translates to the allocated public IP, then recreates packet with new public IP for device. Same thing back going from public to private.

Private devices always have private IP, they are never configured with the public IP.

Dynamic NAT - 1 private to 1st available public.
No static allocation. You have a pool of public IPs to use, allocated as needed. Used when you have a large number of private IPs and want them all to have internet access via public IPs, but when you have less public IPs than private. 

Similar to static, but you have a pool of public IP addresses, usually less than the amount of private devices you have. Public IP will be allocated temporarily and dynamically per device as needed. Same IP can be used by different devices as long as they are not at the same time. If all public IPs are being used, and a new device needs to access out, it can fail.

Port Address Translation (PAT) - many private to 1 public (NATGW)
Where many private addresses are translated onto a single public address. What home internet router normally does. Many devices use PAT, or overloading to use a single public IP. Uses ports to identify devices. PAT is used in AWS.

This uses ports to use just one public IP, but separate machine per source port as a different connection. Source port changes when it reaches Nat device, alongside the IP of course. Then stores IP and Port in NAT TAble. PAcket is changed. 

NAT only works with v4, not v6.

---

## IPv4 Addressing and Subnetting

IPv4 standard created in 1981, RFC791

0.0.0.0 -> 255.255.255.255 = 4.3 billion addresses

Originally directly managed by IANA, Internet Assigned Numbers Authority

Recetnyl, parts of the address space to regional authorities.

IPv4 is allocated, meaning you have to be allocated an IPv4 in order to use them. Can't pick a random one and expect it to work.

Private addresses can be used and reused freely.

Class A space: 0.0.0.0 -> 127.255.255.255
Contains 128 networks. Each has 16.7 mil addresses.
0.anything is reserved.
First octet is network, remaining octets are for hosts or subnetting.
Generally used for huge networks, businesses/orgs. Now have been given up for regional managers in that region. (not iana)

Class B space: 128.0.0.0 -> 191.255.255.255
16384 networks, each containing 65k ips
Typically used for larger businesses that didn't need class A. These are now allocated to the regional authorities. First two octets are the networks. The last two are for the organization to assign to devices or to subnet into smaller networks.

Class C range: 192.0.0.0 -> 223.255.255.255
2 million networks, each containing 256 IPs. First three octets are network, last octet hosts or subnetting. Normally used for smaller businesses, again now are controlled regionally.

Private IP addresses are defined in RFC 1918. Defines three ranges of IPv4 that can be used internally. 

First range is 10.0.0.0 -> 10.255.255.255, total of 16.7m IPs. 1 x Class A Network. Used for cloud environments. Generally broken into smaller subnetworks.

Second range 172.16.0.0 -> 172.31.255.255,  16 x Class B Networks. 65k IPs per network. Generally broken into smaller subnetworks.

Third range 192.168.0.0 -> 192.168.255.255. 256 x Class C networks. 256 IPs. Generally used within home and small office networks. 

---

# Day 3 (3/5)

## Subnetting Pt 2

Subnetting is the process of breaking networks up into smaller pieces.

CIDR (Classless Inter Domain Routing) lets us break down networks. Defines a way of expressing the size of a network, a prefix.

10.16.0.0/16

To split, does it in half, add +1 to /##. so if network is 10.16.0.0/16, you can split into two by doing 10.16.0.0/17. Would then split down the half, 10.16.0.0 -> 10.16.127.255, and then 10.16.128.0 -> 10.16.255.255. You would only add /17 to the main network to signify its been split into two.

---

## DNS

Domain Name System (DNS) 

DNS links names (netflix.com) to IP addresses. We ask DNS what the IP is for the site, then connects to their servers. A huge database that converts. 

DNS Zone, a database
Zonefile, the file storing the zone on disk
DNS Name Server, NS. A dns server which hosts 1 or more zones, and stores 1 or more zonefiles.
Authoritative - contains real/genuine records (boss). Can be trusted
Non-authoritatve/cached - copies of records/zones stored elsewhere to speed things up. 

DNS resolver first

DNS Root - The boss. A zone like any other part of DNS. This zone is hosted on NameServers. DNS Root zone runs on DNS root servers. Its the point every DNS clients knows about and trusts. Queries start here. 

13 Root server IP addresses which host the root zone. Distributed geographically, managed by independent orgs. ICANN operates one of them, NASA, Univ of Maryland. They manage the Hardware. Root zone is managed by IANA, since its IP. 

Root zone does not store much data. Root zone contains high level information on the top level domains, TLDs. Generic TLD such as .com, or country code like .uk. IANA delegates the management of TLDs to other orgs, known as registries.

Root zone points at the registries. 

Root zone points at the name servers hosting the TLD zones run by the registries which are the orgs who manage these TLDs.

### 3/14 (UPDATE CAUSE I FORGOT)

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

------

# Day 4 (3/6)

## Cloud computing

Cloud computing: five characteristics to be cloud:
-on demand self service: you can provision capabilities as needed without requiring human interaction
capabilities = features or products, vm, networking, etc.

-broad network access: capabilities are available over the network and accessed through standard mechanisms

http, https, ssh, rd, vpn

-resource pooling: there is a sense of location independence. no control or knowledge over the exact location of the resources. resources are pooled to serve mutilple consumers using a multi-tenant model

-rapid elasticity: capabilities can be elastically provisioned and released to scale rapidly outward and inward with demand. to the consumer, the capabilities available for provisioning often appear to be unlimited.

-measured service: resource usage can be monitored, controlled, reported and billed

public vs private vs hybrid vs multi cloud

public cloud: cloud environment that is available to the public, aws, azure, google cloud. meet the 5 characteristics above, and are available to the general public

multi-cloud: using multiple cloud environments

private cloud: solution that can be dedicated to your business and ran from business premises. difference between having on-site insfrastructure, vmware, hyperv, etc, and private cloud. 

hybrid cloud uses public and private cloud at the same time.

## Cloud service models

Infrastructure stack: when you deploy an application anywhere, it uses an infrastructure stack. collection of things which that application needs all stacked onto each other. going from warehouse/facilities -> infrastructure -> servers - virtualization -> os -> container -> runtime - data - application

parts managed by you, some by vendor

unit of consumption: what you pay for and what you consume. if you procure a virtual server, unit is virtual machine/OS.

IaaS - infrastructure as a service. facilities, infrastructure, servers, and virtualization are handled by the provider. you consume OS and anything above, containers, runtime, data, application. 

Platform as a service, PAAS. aimed for devs that just have an app they want to run and dont want to worry about anything extra. runtime is consumed.

SAAS: software as a service: consume application only. you pay for app, dont worry about anything else. most apps nowadays.

