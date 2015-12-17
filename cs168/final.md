# CS168 Final Study Guide

## Lookup Table

* ICANN: Internet Corporation for Assigned Names and Numbers
* RIR: Regional Internet Registry. ICANN gives blocks to RIRs.
* ARIN: American Registry for Internet Numbers. Example of a RIR.
* 

* ACL: Access Control List
* BEB: Binary Exponential Backoff
* BIND: (extra) Berkeley Internet Name Domain (most widely used DNS software)
* CSMA/CD: Carrier Sense Multiple Access ( / Collision Detection)
* ECMP: Equal-Cost Multipath
* FCP: Failure-Carrying Packets
* IGMP: Internet Group Management Protocol
* MPLS: Multiprotocol Label Switching
* OSPF: Open Shortest Path First
* SSM: Single Sender Multicast (like CBT, sender is the core)


## Internet Design

* Packet-switching
* Best-effort
* Layering
* Single internetworking layer
* E2E: only implement functionality in lower layer as performance enhancement. Do so only if it doesn't impose burden on applications that don't need it.
* Fate-sharing: Keep state at end hosts instead of routers.


## Physical Layer





## Datalink Layer (Ethernet)

### Multiple Access Control (MAC)

* Channel partitioning: divide channel into pieces
* Taking turns: trading off who gets to transmit
* Random access: allow collisions, then recover

### Random Access

* Carrier Sense: Listen before speaking, and don't interrupt
* Collision detection: If someone else starts talking at the same time, stop; detect by garbled ata transmission
* Randomness: Don't start talking right awy, wait for a random time before trying again

### Aloha Signalling

Sites send packets to hub; if received, hub sends ACK. Collision detected if no ACK, resends with p = 1/N, where N is the number of nodes. Probability of success is 1/e ~ 0.37. Requires synchronization of slots and hubs.

### Slotted ALOHA

All frames same size, time divided into equal slots (time to transmit a frame). Nodes detect collision if multiple nodes transmit, retransmits on each slot with probability p until success. Probabilty of successful transmission: Np(1-p)^(N-1)

### CSMA/CD

* Carrier sense: wait for link to be idle
* Collision detection: listen while transmitting. If collision, abort transmission and send jam signal
* Random access: Binary exponential backoff (BEB). After collision wait, random time before trying again. After mth collision, wait for K*512 bit times before trying again, where K is chosen randomly from 0..2^(m)-1


## Network Layer

### IP

#### Header

IPV4 fragmentation uses DF (don't fragment) and MF (more fragments) flags. Fragment offset uses octets (8-byte units).

IPV6 has expanded addresses, fixed header length, flow label (packets w/ same label should be kept on the same outbound path so not reordered). Removed fragmentation, header-length, checksum, options.

#### Addressing

Classless Interdomain Routing (CIDR): Use slash to represent length of network prefix mask.

Network Address Translation (NAT): Not enough IPV4 addresses. Use port numbers to de/multiplex internal addresses. Changes IP address and port number.


### Multicast

#### Distance-Vector Multicast Routing Protocol (DVMRP)

Find reverse paths from destinations to source, forming a tree. Packets sent along tree, copied when tree splits.

To prune tree, leaf nodes send Non-Membership Report (NMR) if not a member of the tree. If all children of router send NMRs, send prune to parent of router.
Reverse Packet Flooding (RPF): packets broadcast everywhere except incoming port.

#### Core-Based Trees (CBT)

Shared tree built by receiver joins to core. More scalable than DVMRP, no initial flooding. Reduces state saved from O(SG) to O(G).



### Transport Layer

#### TCP/UDP



#### Reliable Transport


#### Congestion Control

(FSM on page 275)




### DNS

DNS servers cache responses to queries along with a TTL field.

#### Record Types

* A: hostname to IP address
* CNAME: name is alias for canonical name
* NS: domain to hostname of nameserver
* PTR: reversed IP quads to corresponding hostname
* MX: domain name to mailserver, includes weight/preference



## Application Layer

### HTTP

#### Overview

Request/response protocol, relies on global URI namespace, stateless, ASCII format

Caching (pull): Replicate content on-demand after a request

Replication (push): planned replication of content in multiple locations

For responses:

* seqnum_res >= acknum_req
* (seqnum_res + payload_res) <= (ack_req + windowsize_req)

#### Multiple Retrieval

* Sequential: One object per sequence
* Concurrent: Parallel connections, unfair to network
* Pipelined: Single connection, multiple requests in pipeline. Good performance and fair to network
* Persistent: Allows TCP connection to be used for later requests


### Interdomain Routing



## Software-Defined Networking


## Network Virtualization


