```
!
! Last configuration change at 14:45:39 MSK Wed May 8 2024
! NVRAM config last updated at 14:45:41 MSK Wed May 8 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname LPR_ASW2
!
boot-start-marker
boot-end-marker
!
!
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
ip cef
no ipv6 cef
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
 no shutdown
 ip address 172.16.2.251 255.255.255.255
 ip ospf 110 area 0
!
interface GigabitEthernet0/0
 no shutdown
 description To-LPR_DSW1
 no switchport
 ip address 172.16.2.10 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/1
 no shutdown
 description To-LPR_ASW1
 no switchport
 ip address 172.16.2.18 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/2
 no shutdown
 description To-LPR_ASW3
 no switchport
 ip address 172.16.2.22 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/3
 no shutdown
 switchport access vlan 4
 switchport mode access
 media-type rj45
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/0
 no shutdown
 switchport access vlan 6
 switchport mode access
 media-type rj45
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/1
 no shutdown
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/2
 no shutdown
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/3
 no shutdown
 media-type rj45
 negotiation auto
!
interface Vlan4
 no shutdown
 ip address 192.168.2.65 255.255.255.224
 ip helper-address 172.16.2.253 
 ip ospf 110 area 0
!
interface Vlan6
 no shutdown
 ip address 192.168.2.129 255.255.255.224
 ip helper-address 172.16.2.253 
 ip ospf 110 area 0
!
router ospf 110
 router-id 4.4.4.4
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
banner exec ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner incoming ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner login ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
!
line con 0
line aux 0
line vty 0 4
 login
!
ntp update-calendar
ntp server 172.16.2.253

```

[Назад](readme.md)