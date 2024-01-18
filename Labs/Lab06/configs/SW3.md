```
Current configuration : 2642 bytes
!
! Last configuration change at 19:54:53 MSK Thu Jan 18 2024
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
no logging console
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
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
track 1 ip sla 1 reachability
 delay down 30 up 30
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
!
interface GigabitEthernet0/0
 description To_SW4
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
 negotiation auto
!
interface GigabitEthernet0/1
 description To_SW5
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
 negotiation auto
!
interface GigabitEthernet0/2
 description To_VPC1
 switchport access vlan 3
 switchport mode access
 load-interval 60
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet0/3
 description Not_Used
 switchport access vlan 111
 switchport mode access
 load-interval 60
 shutdown
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan2
 no ip address
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
ip route 0.0.0.0 0.0.0.0 192.168.3.4 10 track 1
ip route 0.0.0.0 0.0.0.0 192.168.3.5 15
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
ip sla 1
 icmp-echo 192.168.3.4 source-ip 192.168.3.3
 frequency 5
ip sla schedule 1 life forever start-time now
!
!
!
!
control-plane
!
banner login ^CC
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
!
end
```


[Назад](readme.md)

