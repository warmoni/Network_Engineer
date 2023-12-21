```
Building configuration...

Current configuration : 2699 bytes
!
! Last configuration change at 22:24:25 MSK Wed Dec 20 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW9
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$uEDd$UAN9wLzhYYF2.IGcq3y76/
!
no aaa new-model
clock timezone MSK 3 0
!
!
!
!
!
!
ip dhcp excluded-address 10.0.9.1
!
ip dhcp pool HOSTS_VLAN_9
 network 10.0.9.0 255.255.255.0
 default-router 10.0.9.1
 dns-server 1.1.1.1 77.88.8.8
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
 ip address 172.16.3.29 255.255.255.252
 ipv6 address FE80::172:16:3:29 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:28:29/112
 ipv6 enable
!
interface Ethernet0/0
 description Po1.To_SW10
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode on
!
interface Ethernet0/1
 description Po1.To_SW10
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode on
!
interface Ethernet0/2
 description To_VPC8
 switchport access vlan 9
 switchport mode access
 no cdp enable
 spanning-tree portfast edge
!
interface Ethernet0/3
 description To_R17
 no switchport
 ip address 172.16.3.10 255.255.255.252
 duplex auto
 ipv6 address FE80::172:16:3:10 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:8:10/112
 ipv6 enable
!
interface Ethernet1/0
 description To_R16
 no switchport
 ip address 172.16.3.18 255.255.255.252
 duplex auto
 ipv6 address FE80::172:16:3:18 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:16:18/112
 ipv6 enable
!
interface Ethernet1/1
 switchport access vlan 111
 switchport mode access
 shutdown
 no cdp enable
 spanning-tree portfast edge
!
interface Ethernet1/2
 switchport access vlan 111
 switchport mode access
 shutdown
 no cdp enable
 spanning-tree portfast edge
!
interface Ethernet1/3
 switchport access vlan 111
 switchport mode access
 shutdown
 no cdp enable
 spanning-tree portfast edge
!
interface Vlan9
 description Gateway_VLAN_9
 ip address 10.0.9.1 255.255.255.0
 ipv6 address FE80::10:0:9:1 link-local
 ipv6 address 2001:DB8:2042:10:0:9:0:1/96
 ipv6 enable
!
interface Vlan333
 description Management
 ip address 192.168.5.9 255.255.255.0
 ipv6 address FE80::192:168:5:9 link-local
 ipv6 address FD00:3CD5:2042:0:192:168:5:9/112
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
!
line con 0
 password cisco
 logging synchronous
line aux 0
line vty 0 4
 password cisco
 login
 transport input telnet
!
end
```

[Назад](readme.md)