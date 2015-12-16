# CS168 Study Guide

# Designing a Network

## Delay

1. Transmission: how long it takes to push all the bits of a packet into a link (packet size / transmission rate)
2. Propagation: how long it takes to move one bit from one end of the link to the other (link length / propagation speed (speed of light))
3. Processing: hos long a router takes to process a packet
4. Queueing: how long a packet sits in the buffer before it gets processed

## Queuing Theory

L: average number of packets waiting in queue
A: average arrival rate
P: peak arrival rate
W: average time packets wait in the queue

Little's Law: L = A * W

# Designing the Internet

## Layering: 

1. Application: app network support
2. Transport (L4): reliable E2E delivery
3. Network (L3): global best-effort
4. Datalink (L2): local best-effort
5. Physical: bits on wire

bottom 3 layers implemented everywhere, top two implemented at hosts

## E2E principle

Reliability has to be implemented at end hosts, because network is inherently unreliable

## Fate-sharing

When storing state in a distributed system, colocate it with entities that rely on that state; argues that network state should be kept at end hosts rather than inside routers


# Routing Fundamentals

Data plane: forwarding, directing data packet to outgoing link; single router using routing state

Control plane: routing, computing paths packets will follow; inter-router communication to create routing state

Valid routing state: no dead ends, no loops


# Even More Routing

## Distance vector routing

Count to infinity scenario - if a link goes down, further updates will keep going until they hit a max distance

Poisoned reverse: If you're using a next-hop's path, then tell them not to use your path (cost of infinity). Doesn't completely solve count to infinity

Split horizon: if you use a neighbor on a path to x, don't send a routing entry for x to that neighbor

Route poisoning: send infinity when a path is no longer available


# Reliable Transport

Reliability state at end hosts, in L4 layer

Goals: Correctness, timeliness, efficiency, fairness

Transport mechanism is reliable iff it resends all dropped or corrupted packets

Windowing: send W packets, when one gets ACK'ed send the next packet in line.

Cumulative ACK: ACK next expected packet sequence number. Window resent on timeout.


# Designing IP

Security: privacy, integrity, provenance



# Addressing





# Forwarding

Longest Prefix Match (LPM) for IP lookups


# Protocol Support for Forwarding

ARP (address resolution protocol): link layer, maps IP address -> MAC address

if IP not in table, sender broadcasts "who has IP," receiver responds "MAC address x:x:x...," sender caches result in ARP table

DHCP (dynamic host configuration protocol): app layer; used to discover own IP address, netmask, IP addresses for local DNS name servers, IP addresses for first-hop "default" routers

Steps:

1. DHCP discovery message L2 broadcast FF:FF:FF:FF:FF:FF
2. one or more DHCP servers respond with broadcast offer message (proposed IP, lease time)
3. client broadcasts DHCP request message (selected offer, accepted parameters). not-chosen servers learn they were not chosen.
4. Selected DHCP server ACKs (broadcast)

Local broadcast at IP level uses 255.255.255.255


MAC address: hardcoded into network adapter

Ethernet uses learning switches

Construct spanning tree using MAC addresses:

1. Messages (Y, d, X) from node X proposing distance d to root Y
2. Starts with (X, 0, X)
3. Broadcasts to all neighbors if updates



# Transport and TCP


# DNS and HTTP


# ICMP, NAT, and more HTTP


# ICMP, NAT, QoS, and Review


# Congestion Control

Flow control: Keeps one fast sender from overwhelming slow receiver

Congestion control: Keeps set of senders from overwhelming network

## TCP Throughput equation: 

1. Calculate sawtooth time (STT) in terms of RTT
2. Calculate data transferred during STT in terms of MSS
3. Calculate throughput (data / STT)
4. Calculate packet loss (1 MSS / data)
5. Use W to relate packet loss and throughput

Slow-start




# More Congestion Control




# Even More Congestion Control

RED: Random early drop, drop packets with probability depending on average queue size

ECN: Explicit congestion notification (removes confusion with corruption, recovery/rate-adjustment)

RCP: Rate control protocol, packets carry "rate field," routers insert "fair share" value, end hosts adjust to match fair share. Allows instant ramping up.



# Interdomain Routing

## Autonomous Service

Wants freedom of policy, autonomy, privacy; has customers, providers, peers

## Gao-Rexford:

* Topology: Each interAS relationship is either peer-to-peer, or provider-customer. Moreover, the  provider-customer relationships form an acyclic graph (assuming arrows go from Provider to Customer).
* Selection: Each AS prefers Customer paths over Peer paths, and Peer paths over Provider paths.
* Export: Only customer paths are exported to peers and providers. All paths are exported to customers.

## BGP

### Message Types

* Open connection
* Notification (if something's wrong)
* Update (announcements, withdrawals)
* Keepalive (timed pings to indicate alive)

# BGP and Advanced Routing


# Multicast and Routing Research

## Link-Layer Multicast

* Join: NIC also listens for packets sent to multicast address G
* Send: Packet flooded on all LAN segments, like broadcast
* Limitation: Can only flood over LAN

## Network-Layer (IP) Multicast

Inter-network multicast routing, relies on link-layer multicast for intra-network routing

open group membership, send Internet Group Management Protocol (IGMP) message to join
privacy preserved at application layer through encryption

## Distance Vector Multicast Routing Protocol

* Construct a tree from a serouce to all destinations, find reverse-paths from destinations to source
* packets sent along tree, copied when tree splits
* Reverse Path Flooding (RPF): packets broadcast everywhere except incoming port
* Leaf nodes send Non-Membership Report (NMR) towards source if they have no members. If all children of R send NMR, send prune to parent of R
* Requires O(sources*groups) active state

## Core-Based Trees

* More scalable than DVMRP, no initial flooding. Reduces state saved from O(SG) to O(G)
* Shared tree built by receiver joins sent to core

## Failover Routing

Generalization of backup paths; routing based on both destination and incoming port. Have multiple outgoing ports that can be used in the event of failure.

## Multipath

Provide more than one path for each source/destination pair, allow endpoints to pick rather than routers

Necessary because of E2E arguments, but has RTT worth of delay in case of failure

## Failure-Carrying Packets

Each node holds a consistent view of network that might be out of date

Packets carry map number and failure information, used to fix old maps.

When packet arrives and next-hop link for path computed with consistent state is down, insert failure information into packet header

Traffic Engineering (TE): way of distributing load on network (not all packets take shortest path)

Multiprotocol Label Switching (MPLS): Insert label before IP header, route based on label. Edge routers inspect IP header and insert MPLS label, core routers route based on MPLS label

## Routing Along DAGs

Recover from failures without global recomputation, without changes in packet headers, support locally adaptive traffic engineering

Calculating DAG means packets can be sent on any of outgoing links and end up at sink.

If all outgoing links fail, reverse incoming links to outgoing. When an outgoing link fails (or is reversed), do nothing if other outgoing links exist, otherwise reverse all incoming links and change them to outgoing.

Link reversals on control plane, packets can be lost while changes take effect; fix: Data-Driven Connectivity (DDC), define reversal properties in terms of actions that can occur at data speeds

SMPC (Secure multi-party computation), based on Shamir secret-sharing


# Multiple Access and Security

## MAC (Multiple Access Control)

* Channel partitioning: divide channel into pieces
* Taking turns: trading off who gets to transmit
* Random access: allow collisions, then recover

### Random Access

* Carrier Sense: Listen before speaking, and don't interrupt
* Collision detection: If someone else starts talking at the same time, stop; detect by garbled ata transmission
* Randomness: Don't start talking right awy, wait for a random time before trying again

## Aloha Signalling

* Sites send packets to hub; if received, hub sends ACK; if not received, site resends

### Slotted ALOHA

* All frames same size, time divided into equal slots (time to transmit a frame)
* Nodes detect collision if multiple nodes transmit
* On collision, node retransmits on each slot with probability p until success
* Probabilty of successful transmission: Np(1-p)^(N-1)

## CSMA

* Listen before transmitting, doesn't eliminate all collisions (nonzero propagation delay)

## CSMA/CD (Collision Detection)

* Collisions detected and aborted in short time, reducing wastage
* Detection difficult in wireless LANs
* Time wasted in collisions proportional to distance d, time spent transmitting packet is packet length p / bandwidth b
* Efficiency: (p/b) / (p/b + Kd)

## Ethernet Multiple Access

* Carrier sense: wait for link to be idle; collision detection: listen while transmitting
* Random access: binary exponential backoff
    - After collision, wait random time before trying again
    - After mth collision, choose K randomly from 0 to 2^m - 1 and wait for K*512 bit times before trying again
* Binary Exponential Backoff (BEB): After each collision, pick a slot randomly within the next 2^m slots, where m is the number of collisions since the last successful transmission
* MAC Channel capture; finite chance that one connection will never relinquish channel, and other hosts will never send packetx


## Security

* Availability: will network deliver data
* Authentication: who is sending data 
* Integrity: do messages arrive in original form
* Provenance: who created the data (communication may be sent through intermediaries)
* Privacy: can others read my sent data
* Anonymity: can i avoid revealing my identity
* Freedom from traffic analysis: can someone analyze where I am sending from and to whom


# Software-Defined Networking

## Middleboxes


# Rethinking the Internet Architecture


