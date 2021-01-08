# DNS	Lookup	
### Overview

A DNS lookup, in a general sense, is the process by which a DNS record is returned from a DNS server.

Let's use the dig command to do a forward and reverse lookup of ee.t.u-tokyo.ac.jp.

### Forward Lookup
1. Obtain information about the root server. (ftp://rs.internic.net/domain/named.root)

2. Select one of the A records in the above file, and use the dig command and repeat until you get aa (Authoritative Answer: indicating that the response is from an authorized server) in the resulting flag.

```
$ dig @198.41.0.4 ee.t.u-tokyo.ac.jp. A

; <<>> DiG 9.10.6 <<>> @198.41.0.4 ee.t.u-tokyo.ac.jp. A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60079
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 8, ADDITIONAL: 16
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;ee.t.u-tokyo.ac.jp.		IN	A

;; AUTHORITY SECTION:
jp.			172800	IN	NS	a.dns.jp.
jp.			172800	IN	NS	d.dns.jp.
jp.			172800	IN	NS	e.dns.jp.
jp.			172800	IN	NS	f.dns.jp.
jp.			172800	IN	NS	h.dns.jp.
jp.			172800	IN	NS	g.dns.jp.
jp.			172800	IN	NS	c.dns.jp.
jp.			172800	IN	NS	b.dns.jp.

;; ADDITIONAL SECTION:
a.dns.jp.		172800	IN	A	203.119.1.1     ← focus on this!
a.dns.jp.		172800	IN	AAAA	2001:dc4::1
d.dns.jp.		172800	IN	A	210.138.175.244
d.dns.jp.		172800	IN	AAAA	2001:240::53
e.dns.jp.		172800	IN	A	192.50.43.53
e.dns.jp.		172800	IN	AAAA	2001:200:c000::35
f.dns.jp.		172800	IN	A	150.100.6.8
f.dns.jp.		172800	IN	AAAA	2001:2f8:0:100::153
h.dns.jp.		172800	IN	A	65.22.40.25
h.dns.jp.		172800	IN	AAAA	2a01:8840:1ba::25
g.dns.jp.		172800	IN	A	203.119.40.1
c.dns.jp.		172800	IN	A	156.154.100.5
c.dns.jp.		172800	IN	AAAA	2001:502:ad09::5
b.dns.jp.		172800	IN	A	202.12.30.131
b.dns.jp.		172800	IN	AAAA	2001:dc2::1

;; Query time: 58 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Fri Jan 08 21:18:53 JST 2021
;; MSG SIZE  rcvd: 503

$ dig @203.119.1.1 ee.t.u-tokyo.ac.jp A

; <<>> DiG 9.10.6 <<>> @203.119.1.1 ee.t.u-tokyo.ac.jp A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32857
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 4, ADDITIONAL: 6
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;ee.t.u-tokyo.ac.jp.		IN	A

;; AUTHORITY SECTION:
u-tokyo.ac.jp.		86400	IN	NS	dns2.nc.u-tokyo.ac.jp.
u-tokyo.ac.jp.		86400	IN	NS	dns1.nc.u-tokyo.ac.jp.
u-tokyo.ac.jp.		86400	IN	NS	dns3.nc.u-tokyo.ac.jp.
u-tokyo.ac.jp.		86400	IN	NS	dns-x.sinet.ad.jp.

;; ADDITIONAL SECTION:
dns1.nc.u-tokyo.ac.jp.	86400	IN	A	133.11.0.1
dns2.nc.u-tokyo.ac.jp.	86400	IN	A	219.166.230.42
dns3.nc.u-tokyo.ac.jp.	86400	IN	A	157.82.0.1      ← focus on this!
dns1.nc.u-tokyo.ac.jp.	86400	IN	AAAA	2001:200:180::35:1
dns3.nc.u-tokyo.ac.jp.	86400	IN	AAAA	2001:200:180::35:3

;; Query time: 94 msec
;; SERVER: 203.119.1.1#53(203.119.1.1)
;; WHEN: Fri Jan 08 21:19:06 JST 2021
;; MSG SIZE  rcvd: 240

$ dig @157.82.0.1 ee.t.u-tokyo.ac.jp A

; <<>> DiG 9.10.6 <<>> @157.82.0.1 ee.t.u-tokyo.ac.jp A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50156
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 4, ADDITIONAL: 5
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;ee.t.u-tokyo.ac.jp.		IN	A

;; AUTHORITY SECTION:
ee.t.u-tokyo.ac.jp.	60	IN	NS	ns2.t.u-tokyo.ac.jp.
ee.t.u-tokyo.ac.jp.	60	IN	NS	maxwell.ee.t.u-tokyo.ac.jp.
ee.t.u-tokyo.ac.jp.	60	IN	NS	escargot.ee.t.u-tokyo.ac.jp.
ee.t.u-tokyo.ac.jp.	60	IN	NS	ns1.t.u-tokyo.ac.jp.

;; ADDITIONAL SECTION:
ns1.t.u-tokyo.ac.jp.	60	IN	A	133.11.100.151      ← focus on this!
ns2.t.u-tokyo.ac.jp.	60	IN	A	133.11.100.152
maxwell.ee.t.u-tokyo.ac.jp. 60	IN	A	157.82.13.243
escargot.ee.t.u-tokyo.ac.jp. 60	IN	A	133.11.156.18

;; Query time: 126 msec
;; SERVER: 157.82.0.1#53(157.82.0.1)
;; WHEN: Fri Jan 08 21:19:14 JST 2021
;; MSG SIZE  rcvd: 192

$ dig @133.11.100.151 ee.t.u-tokyo.ac.jp A

; <<>> DiG 9.10.6 <<>> @133.11.100.151 ee.t.u-tokyo.ac.jp A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3596
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;ee.t.u-tokyo.ac.jp.		IN	A

;; ANSWER SECTION:
ee.t.u-tokyo.ac.jp.	86400	IN	A	157.82.13.243

;; Query time: 53 msec
;; SERVER: 133.11.100.151#53(133.11.100.151)
;; WHEN: Fri Jan 08 21:19:23 JST 2021
;; MSG SIZE  rcvd: 63
```

### Reverse Lookup

1. Create an FQDN to be searched, which is a string consisting of each octet of <ee.t.u-tokyo.ac.jp's A> finally obtained in Experiment A in reverse order and in-addr.arpa. which indicates a reverse domain to an Internet address.

2. Execute the command as follows.

```
$ dig @198.41.0.4 243.13.82.157.in-addr.arpa PTR

; <<>> DiG 9.10.6 <<>> @198.41.0.4 243.13.82.157.in-addr.arpa PTR
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28437
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 6, ADDITIONAL: 13
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;243.13.82.157.in-addr.arpa.	IN	PTR

;; AUTHORITY SECTION:
in-addr.arpa.		172800	IN	NS	e.in-addr-servers.arpa.
in-addr.arpa.		172800	IN	NS	f.in-addr-servers.arpa.
in-addr.arpa.		172800	IN	NS	d.in-addr-servers.arpa.
in-addr.arpa.		172800	IN	NS	c.in-addr-servers.arpa.
in-addr.arpa.		172800	IN	NS	b.in-addr-servers.arpa.
in-addr.arpa.		172800	IN	NS	a.in-addr-servers.arpa.

;; ADDITIONAL SECTION:
e.in-addr-servers.arpa.	172800	IN	A	203.119.86.101
e.in-addr-servers.arpa.	172800	IN	AAAA	2001:dd8:6::101
f.in-addr-servers.arpa.	172800	IN	A	193.0.9.1
f.in-addr-servers.arpa.	172800	IN	AAAA	2001:67c:e0::1
d.in-addr-servers.arpa.	172800	IN	A	200.10.60.53
d.in-addr-servers.arpa.	172800	IN	AAAA	2001:13c7:7010::53
c.in-addr-servers.arpa.	172800	IN	A	196.216.169.10
c.in-addr-servers.arpa.	172800	IN	AAAA	2001:43f8:110::10
b.in-addr-servers.arpa.	172800	IN	A	199.253.183.183
b.in-addr-servers.arpa.	172800	IN	AAAA	2001:500:87::87
a.in-addr-servers.arpa.	172800	IN	A	199.180.182.53      ← focus on this!
a.in-addr-servers.arpa.	172800	IN	AAAA	2620:37:e000::53

;; Query time: 47 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Fri Jan 08 21:35:53 JST 2021
;; MSG SIZE  rcvd: 431

$ dig @199.180.182.53 243.13.82.157.in-addr.arpa PTR

; <<>> DiG 9.10.6 <<>> @199.180.182.53 243.13.82.157.in-addr.arpa PTR
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63691
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 6, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;243.13.82.157.in-addr.arpa.	IN	PTR

;; AUTHORITY SECTION:
157.in-addr.arpa.	86400	IN	NS	arin.authdns.ripe.net.
157.in-addr.arpa.	86400	IN	NS	y.arin.net.
157.in-addr.arpa.	86400	IN	NS	x.arin.net.     ← focus on this!
157.in-addr.arpa.	86400	IN	NS	r.arin.net.
157.in-addr.arpa.	86400	IN	NS	z.arin.net.
157.in-addr.arpa.	86400	IN	NS	u.arin.net.

;; Query time: 238 msec
;; SERVER: 199.180.182.53#53(199.180.182.53)
;; WHEN: Fri Jan 08 21:36:12 JST 2021
;; MSG SIZE  rcvd: 175

$ dig @x.arin.net. 243.13.82.157.in-addr.arpa PTR

; <<>> DiG 9.10.6 <<>> @x.arin.net. 243.13.82.157.in-addr.arpa PTR
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4235
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 3, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;243.13.82.157.in-addr.arpa.	IN	PTR

;; AUTHORITY SECTION:
82.157.in-addr.arpa.	172800	IN	NS	dns2.nc.u-tokyo.ac.jp.      ← focus on this!
82.157.in-addr.arpa.	172800	IN	NS	dns3.nc.u-tokyo.ac.jp.
82.157.in-addr.arpa.	172800	IN	NS	dns1.nc.u-tokyo.ac.jp.

;; Query time: 232 msec
;; SERVER: 2001:500:f0::63#53(2001:500:f0::63)
;; WHEN: Fri Jan 08 21:36:25 JST 2021
;; MSG SIZE  rcvd: 128

$ dig @dns2.nc.u-tokyo.ac.jp. 243.13.82.157.in-addr.arpa PTR

; <<>> DiG 9.10.6 <<>> @dns2.nc.u-tokyo.ac.jp. 243.13.82.157.in-addr.arpa PTR
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50305
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 2
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;243.13.82.157.in-addr.arpa.	IN	PTR

;; AUTHORITY SECTION:
13.82.157.in-addr.arpa.	86400	IN	NS	ns1.t.u-tokyo.ac.jp.

;; ADDITIONAL SECTION:
ns1.t.u-tokyo.ac.jp.	60	IN	A	133.11.100.151      ← focus on this!

;; Query time: 79 msec
;; SERVER: 219.166.230.42#53(219.166.230.42)
;; WHEN: Fri Jan 08 21:36:35 JST 2021
;; MSG SIZE  rcvd: 104

$ dig @133.11.100.151 243.13.82.157.in-addr.arpa PTR

; <<>> DiG 9.10.6 <<>> @133.11.100.151 243.13.82.157.in-addr.arpa PTR
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65443
;; flags: qr aa rd; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;243.13.82.157.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
243.13.82.157.in-addr.arpa. 3600 IN	PTR	maxwell.ee.t.u-tokyo.ac.jp.
243.13.82.157.in-addr.arpa. 3600 IN	PTR	eeis.t.u-tokyo.ac.jp.
243.13.82.157.in-addr.arpa. 3600 IN	PTR	ccs.t.u-tokyo.ac.jp.
243.13.82.157.in-addr.arpa. 3600 IN	PTR	eegw.ee.t.u-tokyo.ac.jp.
243.13.82.157.in-addr.arpa. 3600 IN	PTR	ee.t.u-tokyo.ac.jp.

;; Query time: 70 msec
;; SERVER: 133.11.100.151#53(133.11.100.151)
;; WHEN: Fri Jan 08 21:36:48 JST 2021
;; MSG SIZE  rcvd: 180
```
