
### Задание 1 

```
==========sw1.txt==========

-spanning-tree mode pvst
+spanning-tree mode rapid-pvst
+spanning-tree vlan 2,4,6,8,10,12,34,78,144,456 priority 0

 vlan 67
  name VLAN67
- shutdown

-vlan 109,144,201-203,211-213,456
+vlan 109,144,201-203,211-213,235,456

 interface Ethernet0/0
- switchport trunk allowed vlan 1-10,23,34,67,78,89,109
+ switchport trunk allowed vlan 1-10,12,23,34,67,78,89,109
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet0/1
  switchport trunk allowed vlan 1-10,12,34,67,78,89,144,235
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet0/2
  switchport trunk allowed vlan 1-10,12,23,34,67,78,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet0/3
  switchport trunk allowed vlan 1-10,12,23,34,78,89,109,201-203,211-213
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet1/0
- switchport trunk allowed vlan 1-10,12,23,34,67,78,89,144,201-203,211-213
+ switchport trunk allowed vlan 1-10,12,23,34,67,78,89,109,144,201-203,211-213
  switchport trunk allowed vlan add 235,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

==========sw2.txt==========

-spanning-tree mode pvst
+spanning-tree mode rapid-pvst

+vlan 89

 vlan 235
  name VLAN235
- state suspend

 interface Ethernet0/0
  switchport trunk allowed vlan 1-10,12,23,34,144,235,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet0/1
  switchport trunk allowed vlan 1-10,12,23,34,67,78,89,109
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

==========sw3.txt==========

-spanning-tree mode pvst
+spanning-tree mode rapid-pvst
+spanning-tree vlan 1,3,5,7,9,23,67,89,109,235 priority 0
+spanning-tree vlan 2,4,6,8,10,12,34,78,144,456 priority 4096

 interface Ethernet0/0
  switchport trunk allowed vlan 1-10,12,23,34,89,144,235
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

==========sw4.txt==========

-spanning-tree mode pvst
+spanning-tree mode rapid-pvst

 interface Ethernet0/0
  switchport trunk allowed vlan 1-10,12,23,34,67,109,144,235,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ spanning-tree portfast edge trunk

 interface Ethernet0/1
  switchport trunk allowed vlan 1-10,12,67,78,109,144
  switchport trunk encapsulation dot1q
  switchport mode trunk
- shutdown
+ spanning-tree portfast edge trunk
```


### Задание 2 

```
==========sw1.txt==========

-vtp mode transparent
+vtp file nvram:ccie.dat
+vtp domain SW1
+vtp mode off

==========sw2.txt==========

-vtp mode transparent

+vtp file nvram:ccie.dat
+vtp domain SW2
+vtp mode off

==========sw3.txt==========

-vtp mode transparent

+vtp file nvram:ccie.dat
+vtp domain SW3
+vtp mode off

==========sw4.txt==========

-vtp mode transparent

+vtp file nvram:ccie.dat
+vtp domain SW4
+vtp mode off
```


### Задание 3 

```
==========r1.txt==========

+ip ssh port 3009 rotary 9

+ip ssh version 2

 line vty 3
- login
- transport input none
- transport output none
+ login local
+ rotary 9
+ autocommand  ssh -l ccie -p 9009 180.1.7.7
+ transport input ssh
+ transport output ssh

==========r7.txt==========

+username ccie nopassword

+ip ssh port 9009 rotary 9
+ip ssh version 2

 line vty 3
- login
- transport input none
- transport output none
+ login local
+ rotary 9
+ transport input ssh
```


### Задание 4 

Настроить между r9 и r10 в сети интерфейса e0/1.109 RIPv2 таким образом, чтобы обновления между ними отправлялись только на directed broadcast адрес сети.
```
==========r9.txt==========

 interface Ethernet0/1.109
  encapsulation dot1Q 109
  ip address 188.1.109.9 255.255.255.0
+ ip broadcast-address 188.1.109.255
+ ip rip v2-broadcast

==========r10.txt==========

 interface Ethernet0/1.109
  encapsulation dot1Q 109
  ip address 188.1.109.10 255.255.255.0
+ ip broadcast-address 188.1.109.255
+ ip rip v2-broadcast
```

Восстановить обмен маршрутами RIPv2 между r8 и r9.

Кроме того, с Loopback0 r8 должен пинговаться с loopback0 r9.
```
==========r8.txt==========

 router rip
  version 2
+ no validate-update-source
  network 180.1.0.0
  network 188.1.0.0
  distribute-list 78 out Ethernet0/1.78
  no auto-summary

+ip route 188.1.98.9 255.255.255.255 Ethernet0/1.89

==========r9.txt==========

 router rip
  version 2
+ no validate-update-source
  network 180.1.0.0
  network 188.1.0.0
  no auto-summary

+ip route 188.1.89.8 255.255.255.255 Ethernet0/1.89
```

TSHOOT:
```
 ip access-list extended C-3PO
+ permit udp any any eq rip
+ permit udp any eq rip any
```


### Задание 5 
```
==========r6.txt==========

+no service dhcp
+no logging on

 line con 0
  exec-timeout 0 0
  privilege level 15
  logging synchronous
+ escape-character 3
```


### Задание 6 

```
==========r9.txt==========

+logging source-interface Loopback0
+logging host 180.1.7.7 transport tcp port 10001

==========r7.txt==========

-no ip http server
+ip http server
+ip http port 10001
```


### Задание 7 

```
==========r5.txt==========

 interface Ethernet0/1.235
  encapsulation dot1Q 235
+ ip address dhcp

==========r2.txt==========

 interface Ethernet0/1.235
  encapsulation dot1Q 235
  ip address 188.1.235.2 255.255.255.0
+ ip helper-address 180.1.7.7

==========r3.txt==========

 interface Ethernet0/1.235
  encapsulation dot1Q 235
  ip address 188.1.235.3 255.255.255.0
  ip access-group Darth_Vader in
+ ip helper-address 180.1.7.7

==========r7.txt==========

+ip dhcp excluded-address 188.1.235.1 188.1.235.4
+ip dhcp excluded-address 188.1.235.6 188.1.235.255

+ip dhcp pool VLAN235
+ network 188.1.235.0 255.255.255.0
+ default-router 188.1.235.2
```
TSHOOT:
```
==========r3.txt==========

 ip access-list extended Darth_Vader
+ permit udp any any eq bootps
  deny   icmp any host 180.1.5.5
  deny   icmp any host 180.1.2.2
  deny   icmp any host 180.1.1.1
  deny   udp any any eq bootps
  deny   tcp any any eq 22
  permit ip any any
```


### Задание 8 

```
==========sw1.txt==========

+ip dhcp snooping vlan 235
+ip dhcp snooping information option allow-untrusted
+ip dhcp snooping

 interface Ethernet0/1
  switchport trunk allowed vlan 1-10,12,34,67,78,89,144,235
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ ip dhcp snooping trust

 interface Ethernet3/0
  switchport trunk allowed vlan 1-10,12,23,34,67,78,89,109,144,235,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport nonegotiate
+ ip dhcp snooping trust

==========sw2.txt==========

+ip dhcp snooping vlan 235
+ip dhcp snooping

 interface Ethernet2/2
  switchport trunk allowed vlan 1-10,12,23,34,67,78,89,109,144,235,456
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport nonegotiate
+ ip dhcp snooping trust

==========sw3.txt==========

+ip dhcp snooping vlan 235
+ip dhcp snooping information option allow-untrusted
+ip dhcp snooping

 interface Ethernet0/0
  switchport trunk allowed vlan 1-10,12,23,34,89,144,235
  switchport trunk encapsulation dot1q
  switchport mode trunk
+ ip dhcp snooping trust

==========r2.txt==========

+ip dhcp relay information trust-all

==========r3.txt==========

+ip dhcp relay information trust-all
```


### Задание 9 

```
==========r2.txt==========
+ip route 180.1.0.0 255.255.0.0 188.1.235.3
+ip route 180.1.1.1 255.255.255.255 188.1.12.1

==========r3.txt==========

 interface Ethernet0/1.144
  encapsulation dot1Q 144
  ip address 144.1.0.3 255.255.255.0
- shutdown

+ip route 180.1.0.0 255.255.252.0 188.1.235.2

==========r4.txt==========
+ip route 180.1.1.1 255.255.255.255 188.1.34.3

==========r6.txt==========
+ip route 180.1.7.7 255.255.255.255 188.1.67.7

==========r7.txt==========
+ip route 180.1.0.0 255.255.248.0 188.1.100.10
+ip route 180.1.6.6 255.255.255.255 188.1.67.6
```


### Задание 10 ###

```
==========r2.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-all NON_DEFAULT
+ match not ip dscp default

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map OUT
+ class BS_ALL
+  shape average percent 50
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 50
+   service-policy ENT_QOS

 interface Ethernet0/1.235
  encapsulation dot1Q 235
  ip address 188.1.235.2 255.255.255.0
+ service-policy output OUT

+ip access-list extended BS_NET
+ permit ip 188.1.201.0 0.0.0.255 any
+ permit ip 188.1.202.0 0.0.0.255 any
+ permit ip 188.1.203.0 0.0.0.255 any

+ip access-list extended ENT_NET
+ permit ip 188.1.211.0 0.0.0.255 any
+ permit ip 188.1.212.0 0.0.0.255 any
+ permit ip 188.1.213.0 0.0.0.255 any

==========r3.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-any NON_DEFAULT
+ match not ip dscp default

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map OUT
+ class BS_ALL
+  shape average percent 60
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 40
+   service-policy ENT_QOS

 interface Ethernet0/1.235
  encapsulation dot1Q 235
  ip address 188.1.235.3 255.255.255.0
  ip access-group Darth_Vader in
+ service-policy output OUT

+ip access-list extended BS_NET
+ permit ip 188.1.201.0 0.0.0.255 any
+ permit ip 188.1.202.0 0.0.0.255 any
+ permit ip 188.1.203.0 0.0.0.255 any

+ip access-list extended ENT_NET
+ permit ip 188.1.211.0 0.0.0.255 any
+ permit ip 188.1.212.0 0.0.0.255 any
+ permit ip 188.1.213.0 0.0.0.255 any

==========r4.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-all NON_DEFAULT
+ match not ip dscp default

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map OUT
+ class BS_ALL
+  shape average percent 70
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 30
+   service-policy ENT_QOS

 interface Ethernet0/1
  ip address 10.0.0.4 255.255.255.0
+ hold-queue 2000 out

 interface Ethernet0/1.34
  encapsulation dot1Q 34
  ip address 188.1.34.4 255.255.255.0
+ service-policy output OUT

+ip access-list extended BS_NET
+ permit ip 188.1.201.0 0.0.0.255 any
+ permit ip 188.1.202.0 0.0.0.255 any
+ permit ip 188.1.203.0 0.0.0.255 any

+ip access-list extended ENT_NET
+ permit ip 188.1.211.0 0.0.0.255 any
+ permit ip 188.1.212.0 0.0.0.255 any
+ permit ip 188.1.213.0 0.0.0.255 any

==========r7.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-all NON_DEFAULT
+ match not ip dscp default

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map OUT72
+ class BS_ALL
+  shape average percent 50
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 50
+   service-policy ENT_QOS

+policy-map OUT73
+ class BS_ALL
+  shape average percent 60
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 40
+   service-policy ENT_QOS

+policy-map OUT74
+ class BS_ALL
+  shape average percent 70
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 30
+   service-policy ENT_QOS

+policy-map TUN_OUT74
+ class class-default
+  shape average percent 100
+   service-policy OUT74

+policy-map TUN_OUT72
+ class class-default
+  shape average percent 100
+   service-policy OUT72

+policy-map TUN_OUT73
+ class class-default
+  shape average percent 100
+   service-policy OUT73

 interface Tunnel72
  ip address 188.1.100.9 255.255.255.252
  tunnel source Ethernet0/1.144
  tunnel destination 144.1.0.2
+ service-policy output TUN_OUT72

 interface Tunnel73
  ip address 188.1.100.5 255.255.255.252
  tunnel source Ethernet0/1.144
  tunnel destination 144.1.0.3
+ service-policy output TUN_OUT73

 interface Tunnel74
  ip address 188.1.100.1 255.255.255.252
  tunnel source Ethernet0/1.144
  tunnel destination 144.1.0.4
+ service-policy output TUN_OUT74

 interface Ethernet0/1
  ip address 10.0.0.7 255.255.255.0
  ip access-group C-3PO in
+ hold-queue 5000 out

+ip access-list extended BS_NET
+ permit ip 188.1.201.0 0.0.0.255 any
+ permit ip 188.1.202.0 0.0.0.255 any
+ permit ip 188.1.203.0 0.0.0.255 any


+ip access-list extended ENT_NET
+ permit ip 188.1.211.0 0.0.0.255 any
+ permit ip 188.1.212.0 0.0.0.255 any
+ permit ip 188.1.213.0 0.0.0.255 any

==========r8.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-all NON_DEFAULT
+ match not ip dscp default

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map OUT
+ class BS_ALL
+  shape average percent 70
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 30
+   service-policy ENT_QOS

 interface Ethernet0/1.78
  encapsulation dot1Q 78
  ip address 188.1.78.8 255.255.255.0
  ip summary-address rip 188.1.64.0 255.255.192.0
  ip summary-address rip 180.1.0.0 255.255.240.0
+ service-policy output OUT

==========r9.txt==========

+class-map match-all AF41
+ match ip dscp af41

+class-map match-all EF
+ match ip dscp ef

+class-map match-all AF21
+ match ip dscp af21

+class-map match-all AF31
+ match ip dscp af31

+class-map match-all AF32
+ match ip dscp af32

+class-map match-all AF11
+ match ip dscp af11

+class-map match-all BS_ALL
+ match access-group name BS_NET

+class-map match-any ENT_VLAN
+ match vlan  211
+ match vlan  212
+ match vlan  213

+class-map match-all ENT_ALL
+ match access-group name ENT_NET

+class-map match-any BS_VLAN
+ match vlan  201
+ match vlan  202
+ match vlan  203

+class-map match-all NON_DEFAULT
+ match not ip dscp default

+class-map match-all ENT_VOIP
+ match class-map ENT_VLAN
+ match access-group name VOIP

+class-map match-all ENT_ICMP
+ match class-map ENT_VLAN
+ match access-group name ICMP

+class-map match-all ENT_HTTP
+ match class-map ENT_VLAN
+ match access-group name HTTP

+class-map match-all BS_VOIP
+ match class-map BS_VLAN
+ match access-group name VOIP

+class-map match-all BS_HTTP
+ match class-map BS_VLAN
+ match access-group name HTTP

+class-map match-all BS_ICMP
+ match class-map BS_VLAN
+ match access-group name ICMP

+class-map match-all ENT_NON_DEFAULT
+ match class-map ENT_VLAN
+ match not ip dscp default

+policy-map ENT_QOS
+ class AF41
+  priority percent 10
+ class AF32
+  priority percent 5
+ class AF11
+  bandwidth percent 35
+ class class-default
+  random-detect dscp-based

+policy-map IN_QOS
+ class BS_VOIP
+  set ip dscp ef
+ class BS_HTTP
+  set ip dscp af31
+ class BS_ICMP
+  set ip dscp af21
+ class ENT_VOIP
+  set ip dscp af41
+ class ENT_HTTP
+  set ip dscp af32
+ class ENT_ICMP
+  set ip dscp af11
+ class ENT_NON_DEFAULT
+  set ip dscp default

+policy-map BS_QOS
+ class EF
+  priority percent 10
+ class AF31
+  priority percent 5
+ class AF21
+  bandwidth percent 35
+ class NON_DEFAULT
+  bandwidth percent 25
+ class class-default
+  random-detect dscp-based

+policy-map OUT
+ class BS_ALL
+  shape average percent 70
+   service-policy BS_QOS
+ class ENT_ALL
+  shape average percent 30
+   service-policy ENT_QOS

 interface Ethernet0/1
  ip address 10.0.0.9 255.255.255.0
+ service-policy input IN_QOS

 interface Ethernet0/1.89
  encapsulation dot1Q 89
  ip address 188.1.98.9 255.255.255.0
+ service-policy output OUT
```
