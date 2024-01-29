```
Current configuration : 4302 bytes
!
! Last configuration change at 14:48:06 MSK Mon Jan 29 2024
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
ipv6 dhcp pool HOSTS_VLAN_9v6
 address prefix 2001:DB8:2042:10:0:9::/96
 dns-server 2001:DB8:2042:10:0:9:0:1
!
ipv6 multicast-routing
ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
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
interface Loopback1
 description Management
 ip address 192.168.254.9 255.255.255.255
 ipv6 address FE80::192:168:254:9 link-local
 ipv6 address FD00:3CD5:2042:0:192:168:254:9/128
 ipv6 enable
!
interface Port-channel1
 no switchport
 ip address 172.16.3.29 255.255.255.252
 ipv6 address FE80::172:16:3:29 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:28:29/112
 ipv6 enable
!
interface GigabitEthernet0/0
 description Po1.To_SW10
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet0/1
 description Po1.To_SW10
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet0/2
 description To_VPC8
 switchport access vlan 9
 switchport mode access
 media-type rj45
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet0/3
 description To_R17
 no switchport
 ip address 172.16.3.10 255.255.255.252
 negotiation auto
 ipv6 address FE80::172:16:3:10 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:8:10/112
 ipv6 enable
!
interface GigabitEthernet1/0
 description To_R16
 no switchport
 ip address 172.16.3.18 255.255.255.252
 negotiation auto
 ipv6 address FE80::172:16:3:18 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:16:18/112
 ipv6 enable
!
interface GigabitEthernet1/1
 description To_Ubuntu
 switchport access vlan 9
 switchport mode access
 media-type rj45
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet1/2
 switchport access vlan 111
 switchport mode access
 shutdown
 media-type rj45
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
!
interface GigabitEthernet1/3
 switchport access vlan 111
 switchport mode access
 shutdown
 media-type rj45
 negotiation auto
 no cdp enable
 spanning-tree portfast edge
!
interface Vlan9
 description Gateway_VLAN_9
 ip address 10.0.9.1 255.255.255.0
 ipv6 address FE80::10:0:9:1 link-local
 ipv6 address 2001:DB8:2042:10:0:9:0:1/96
 ipv6 enable
 ipv6 nd managed-config-flag
 ipv6 dhcp server HOSTS_VLAN_9v6
!
!
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.0.9.1 0.0.0.0
  network 172.16.3.10 0.0.0.0
  network 172.16.3.18 0.0.0.0
  network 172.16.3.29 0.0.0.0
  network 192.168.254.9 0.0.0.0
  eigrp router-id 9.9.9.9
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 9.9.9.9
 exit-address-family
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
!
end
```

[Назад](readme.md)
