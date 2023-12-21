```
Building configuration...

Current configuration : 2867 bytes
!
! Last configuration change at 15:18:29 MSK Thu Dec 21 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$6.mN$mFAsGhljSC7PucNiDH03m/
!
no aaa new-model
clock timezone MSK 3 0
!
!
!
!
!
!
!
!
no ip domain-lookup
ip cef
ipv6 unicast-routing
ipv6 multicast-routing
ipv6 cef
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 description To_SW4
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
!
interface Ethernet0/1
 description To_SW5
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
!
interface Ethernet0/2
 description To_VPC1
 switchport access vlan 3
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Ethernet0/3
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Ethernet1/0
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Ethernet1/1
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Ethernet1/2
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Ethernet1/3
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 spanning-tree portfast
 spanning-tree bpdufilter enable
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan333
 description Management
 ip address 192.168.3.3 255.255.255.248
 ipv6 address FE80::192:168:3:3 link-local
 ipv6 address FD00:3CD5:1001:0:192:168:3:3/112
 ipv6 enable
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
banner login ^C
*************************************************************
**          All activity is subject to monitoring.         **
**      Any UNAUTHORIZED access or use is PROHIBITED,      **
**              and may result in PROSECUTION.             **
*************************************************************
^C
!
line con 0
 password 7 045802150C2E
 logging synchronous
line aux 0
line vty 0 4
 password 7 14141B180F0B
 login
 transport input telnet ssh
!
end
```

[Назад](readme.md)