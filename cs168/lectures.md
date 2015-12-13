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

Link-layer multicast: 



# Multiple Access and Security


# Software-Defined Networking


# Rethinking the Internet Architecture


