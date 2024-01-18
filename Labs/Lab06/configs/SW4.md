```
Current configuration : 4914 bytes
!
! Last configuration change at 19:29:01 MSK Thu Jan 18 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW4
!
boot-start-marker
boot-end-marker
!
!
no logging console
enable secret 5 $1$bFNn$PDX15t8j/6ER3HXJvJHAi/
!
no aaa new-model
clock timezone MSK 3 0
!
!
!
!
!
!
ip dhcp excluded-address 10.0.3.1 10.0.3.3
ip dhcp excluded-address 10.0.2.1 10.0.2.128
ip dhcp excluded-address 10.0.3.128 10.0.3.254
!
ip dhcp pool HOSTS_VLAN_2
 network 10.0.2.0 255.255.255.0
 default-router 10.0.2.1
 dns-server 1.1.1.1 77.88.8.8
!
ip dhcp pool HOSTS_VLAN_3
 network 10.0.3.0 255.255.255.0
 dns-server 1.1.1.1 77.88.8.8
 default-router 10.0.3.1
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
interface Port-channel1
 no switchport
 ip address 172.16.1.41 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 10
 ipv6 address FE80::172:16:1:41 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:40:41/112
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network point-to-point
!
interface GigabitEthernet0/0
 description To_SW3
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
 negotiation auto
!
interface GigabitEthernet0/1
 description To_SW2
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
 negotiation auto
!
interface GigabitEthernet0/2
 description Po1.To_SW5
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet0/3
 description Po1.To_SW5
 no switchport
 no ip address
 speed 1000
 duplex full
 no negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet1/0
 description To_R12
 no switchport
 ip address 172.16.1.34 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 10
 negotiation auto
 ipv6 address FE80::172:16:1:34 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:32:34/112
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network point-to-point
!
interface GigabitEthernet1/1
 description To_R13
 no switchport
 ip address 172.16.1.30 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 10
 negotiation auto
 ipv6 address FE80::172:16:1:30 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:28:30/112
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network point-to-point
!
interface GigabitEthernet1/2
 description Not_Used
 switchport access vlan 111
 switchport mode access
 shutdown
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
!
interface GigabitEthernet1/3
 description Not_Used
 switchport access vlan 111
 switchport mode access
 shutdown
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
!
interface Vlan2
 description Gateway_VLAN_2
 ip address 10.0.2.3 255.255.255.0
 standby version 2
 standby 2 ipv6 FE80::10:0:2:1
 standby 2 ipv6 2001:DB8:1001:10:0:2:0:1/96
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:2:3 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 2 description Gateway_VLAN_2
 vrrp 2 ip 10.0.2.1
!
interface Vlan3
 description Gateway_VLAN_3
 ip address 10.0.3.2 255.255.255.0
 standby version 2
 standby 3 ipv6 FE80::10:0:3:1
 standby 3 ipv6 2001:DB8:1001:10:0:3:0:1/96
 standby 3 priority 101
 standby 3 preempt
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:3:2 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 3 description Gateway_VLAN_3
 vrrp 3 ip 10.0.3.1
 vrrp 3 priority 101
!
interface Vlan333
 description Management
 ip address 192.168.3.4 255.255.255.248
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::192:168:3:4 link-local
 ipv6 address FD00:3CD5:1001:0:192:168:3:4/112
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
!
router ospf 110
 router-id 4.4.4.4
 area 10 stub
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
ipv6 router ospf 110
 router-id 4.4.4.4
 area 10 stub
!
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
 password 7 14141B180F0B
 logging synchronous
line aux 0
line vty 0 4
 password 7 030752180500
 login
 transport input telnet ssh
!
!
end
```


[Назад](readme.md)

