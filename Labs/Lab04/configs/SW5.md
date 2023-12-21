```
Building configuration...

Current configuration : 3850 bytes
!
! Last configuration change at 22:41:00 MSK Wed Dec 20 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW5
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$z6C8$2D930XUbKnqYlmghYQjhp1
!
no aaa new-model
clock timezone MSK 3 0
!
!
!
!
!
!
ip dhcp excluded-address 10.0.2.1 10.0.2.3
ip dhcp excluded-address 10.0.2.128 10.0.2.254
ip dhcp excluded-address 10.0.3.1 10.0.3.128
!
ip dhcp pool DHCP_VLAN_2
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
!
interface Port-channel1
 no switchport
 ip address 172.16.1.42 255.255.255.252
 ipv6 address FE80::172:16:1:42 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:40:42/112
 ipv6 enable
!
interface Ethernet0/0
 description To_SW2
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 load-interval 60
!
interface Ethernet0/1
 description To_SW3
 switchport trunk allowed vlan 2,3,333
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 load-interval 60
!
interface Ethernet0/2
 description Po1.To_SW4
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode on
!
interface Ethernet0/3
 description Po1.To_SW4
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode on
!
interface Ethernet1/0
 description To_R13
 no switchport
 ip address 172.16.1.26 255.255.255.252
 duplex auto
 ipv6 address FE80::172:16:1:26 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:24:26/112
 ipv6 enable
!
interface Ethernet1/1
 description To_R12
 no switchport
 ip address 172.16.1.38 255.255.255.252
 duplex auto
 ipv6 address FE80::172:16:1:38 link-local
 ipv6 address FD00:3CD5:1001:172:16:1:36:38/112
 ipv6 enable
!
interface Ethernet1/2
 description Not_Used
 switchport access vlan 111
 switchport mode access
 shutdown
 no cdp enable
 spanning-tree portfast edge
!
interface Ethernet1/3
 description Not_Used
 switchport access vlan 111
 switchport mode access
 shutdown
 no cdp enable
 spanning-tree portfast edge
!
interface Vlan2
 description Gateway_VLAN_2
 ip address 10.0.2.2 255.255.255.0
 standby version 2
 standby 2 ipv6 FE80::10:0:2:1
 standby 2 ipv6 2001:DB8:1001:10:0:2:0:1/96
 standby 2 priority 101
 standby 2 preempt
 ipv6 address FE80::10:0:2:2 link-local
 ipv6 enable
 vrrp 2 description Gateway_VLAN_2
 vrrp 2 ip 10.0.2.1
 vrrp 2 priority 101
!
interface Vlan3
 description Gateway_VLAN_3
 ip address 10.0.3.3 255.255.255.0
 standby version 2
 standby 3 ipv6 FE80::10:0:3:1
 standby 3 ipv6 2001:DB8:1001:10:0:3:0:1/96
 ipv6 address FE80::10:0:3:3 link-local
 ipv6 enable
 vrrp 3 description Gateway_VLAN_3
 vrrp 3 ip 10.0.3.1
!
interface Vlan333
 description Management
 ip address 192.168.3.5 255.255.255.248
 ipv6 address FE80::192:168:3:5 link-local
 ipv6 address FD00:3CD5:1001:0:192:168:3:5/112
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
 password 7 13061E010803
 logging synchronous
line aux 0
line vty 0 4
 password 7 0822455D0A16
 login
 transport input telnet ssh
!
end
```

[Назад](readme.md)