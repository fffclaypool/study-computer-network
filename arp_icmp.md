# ARP Protocol / ICMP Protocol
### Overview
ARP (Address Resolution Protocol) is a protocol that allows us to obtain information about Ethernet MAC addresses from IP addresses.
Since there is no automatic association between IP address and MAC address, it is necessary to use ARP to obtain the MAC address.

ICMP (Internet Control Message Protocol) is a protocol for transferring "error notifications" and "control messages" of the IP protocol.
It is used to check the communication status between computers where TCP/IP is implemented.
The network diagnostic programs ping and traceroute are programs that use this ICMP protocol.

Build a real network and try to capture ARP/ICMP packets.

### Procedure
1. Configure the network based on the configuration diagram 

<img width="844" src="https://github.com/fffclaypool/study_computer_network/blob/images/arp_icmp.png">

2. Execute ping

```
$ ping -c 5 10.1.2.3 
```

3. Check the ARP table

##### host1

| Address  | HWtype | HWaddress         | Flags | Mask | Iface |
| -------- | ------ | ----------------- | ----- | ---- | ----- |
| 10.1.2.3 | ether  | 08:00:27:2e:e1:2b | C     |      | eth0  |      

##### host2

| Address  | HWtype | HWaddress         | Flags | Mask | Iface |
| -------- | ------ | ----------------- | ----- | ---- | ----- |
| 10.0.0.1 | ether  | 08:00:27:fa:3c:ef | C     |      | eth0  | 

4. Check the packet in wireshark.

##### ARP Packet (who has ~ ?)

| Item         | Value      |
| ------------- | ------------- |
| Destination | ffffffffffff |
| Sender Mac Address | 008027fa3cef |
| Type | 0800 |
| Hardware Type | 0001 |
| Protocol Type | 0800 |
| Hardware Size | 06 |
| Protocol Size | 04 |
| Opcode | 0001 |
| Sender Mac Address | 080027fa3cef |
| Sender IP Address | 0a000001 |
| Target Mac Address | 000000000000 |
| Target IP Address | 0a010203 |

##### ARP Packet (~ is at ~)

| Item         | Value      |
| ------------- | ------------- |
| Destination | ffffffffffff |
| Sender Mac Address | 008027fa3cef |
| Type | 0806 |
| Hardware Type | 0001 |
| Protocol Type | 0800 |
| Hardware Size | 06 |
| Protocol Size | 04 |
| Opcode | 0002 |
| Sender Mac Address | 080027fa3cef |
| Sender IP Address | 0a000001 |
| Target Mac Address | 0800272ee12b |
| Target IP Address | 0a010203 |

##### ICMP Packet (Echo Request)

| Item         | Value      |
| ------------- | ------------- |
| Destination | 0800272ee12b |
| Sender Mac Address | 008027fa3cef |
| Type | 0800 |
| Header Length | 45 |
| Differentiated Services Field | 00 |
| Total Length | 0054 |
| Identification | 0000 |
| Fragment Offset | 4000 |
| Time to Live | 40 |
| Protocol | 01 |
| Header Checksum | 24a5 |
| Source | 0a000001 |
| Destination | 0a010203 |
| Type | 08 |
| Code | 00 |
| Checksum | 43ba |
| Identifier | 8e2a |
| Sequence Number | 0002 |