# Longest	Match	First
### Overview
To confirm the Network Layer (Longest Match First) by actually configuring a network in VirtualBox.

### Procedure
1. Configure the network based on the configuration diagram 

2. After the configuration is complete, ping the directly connected nodes to check for communication between them

[ここに画像を貼る]

3. Add a reverse path in HostB

```
hostB$ route add default gw 11.0.0.2	
```

4. Add a default route in HostA

```
hostA$ route add default gw 10.0.0.2

hostA$ route
Kernel IP routing table
Destination   Gateway   Genmask     Flags   Metric    Ref   Use   Iface
default       10.0.0.2  0.0.0.0     UG      0         0     0     eth0
10.0.0.0      *         255.0.0.0   U       0         0     0     eth0

hostA$ traceroute 11.0.0.1
traceroute to 11.0.0.1 (11.0.0.1), 30 hops max, 38 byte packets
1   10.0.0.2(10.0.0.2)   0.673ms    0.629ms   0.676ms
2   11.0.0.1(11.0.0.1)   1.498ms    1.240ms   1.313ms
```

5. Adding a route to network W

```
hostA$ route add -net 11.0.0.0 netmask 255.0.0.0 gw 10.0.0.3

hostA$ route
Kernel IP routing table
Destination   Gateway   Genmask     Flags   Metric    Ref   Use   Iface
default       10.0.0.2  0.0.0.0     UG      0         0     0     eth0
10.0.0.0      *         255.0.0.0   U       0         0     0     eth0
11.0.0.0      10.0.0.3  255.0.0.0   UG      0         0     0     eth0

hostA$ traceroute 11.0.0.1
traceroute to 11.0.0.1 (11.0.0.1), 30 hops max, 38 byte packets
1   10.0.0.3(10.0.0.3)   1.298ms    0.551ms   0.468ms
2   11.0.0.1(11.0.0.1)   1.357ms    1.050ms   0.894ms
```

6. Add a host route to hostB

```
hostA$ route add -host 11.0.0.1 gw 10.0.0.4

hostA$ route
Kernel IP routing table
Destination   Gateway   Genmask           Flags   Metric    Ref   Use   Iface
default       10.0.0.2  0.0.0.0           UG      0         0     0     eth0
10.0.0.0      *         255.0.0.0         U       0         0     0     eth0
11.0.0.0      10.0.0.3  255.0.0.0         UG      0         0     0     eth0
11.0.0.1      10.0.0.4  255.255.255.255   UGH     0         0     0     eth0

hostA$ traceroute 11.0.0.1
traceroute to 11.0.0.1 (11.0.0.1), 30 hops max, 38 byte packets
1   10.0.0.4(10.0.0.4)   2.491ms    0.538ms   0.444ms
2   11.0.0.1(11.0.0.1)   1.313ms    1.158ms   1.002ms
```

7. In parallel with routerX, routerY, and routerZ, another router, routerU, is added and configured appropriately. The route provided by this router is configured in hostA as follows. Also, delete the host route.

```
hostA$ route add -net 11.0.0.0 netmask 255.128.0.0 gw 10.0.0.5
hostA$ route del -host 11.0.0.1
hostA$ route add -host 11.0.0.1 gw 10.0.0.4

hostA$ route
Kernel IP routing table
Destination   Gateway   Genmask           Flags   Metric    Ref   Use   Iface
default       10.0.0.2  0.0.0.0           UG      0         0     0     eth0
10.0.0.0      *         255.0.0.0         U       0         0     0     eth0
11.0.0.0      10.0.0.5  255.128.0.0       UG      0         0     0     eth0
11.0.0.0      10.0.0.3  255.0.0.0         UG      0         0     0     eth0

hostA$ traceroute 11.0.0.1
traceroute to 11.0.0.1 (11.0.0.1), 30 hops max, 38 byte packets
1   10.0.0.5(10.0.0.5)   2.615ms    0.953ms   0.740ms
2   11.0.0.1(11.0.0.1)   2.317ms    1.348ms   0.897ms

```
