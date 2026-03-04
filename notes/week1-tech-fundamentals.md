Day 1

OSI Model Intro:

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

Layer 1: Physical:

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

Layer 2: Data Link

Layer 2 network requires a fuinctional layer 1 network to operate. Hihgher layers build on lower layers adding features and capabilities.

Frames are a format for sending information over a layer 2 network. 

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