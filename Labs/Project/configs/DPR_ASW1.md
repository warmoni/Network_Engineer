```
!
! Last configuration change at 14:16:42 MSK Wed May 8 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DPR_ASW1
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
interface Loopback1
 no shutdown
 ip address 172.16.3.251 255.255.255.255
 ip ospf 110 area 1
!
interface GigabitEthernet0/0
 no shutdown
 description To-DPR_DSW1
 no switchport
 ip address 172.16.3.22 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/1
 no shutdown
 switchport access vlan 2
 switchport mode access
 media-type rj45
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet0/2
 no shutdown
 switchport access vlan 3
 switchport mode access
 media-type rj45
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet0/3
 no shutdown
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/0
 no shutdown
 description To-DPR_DSW2
 no switchport
 ip address 172.16.3.34 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex half
 no negotiation auto
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
interface Vlan2
 no shutdown
 ip address 192.168.3.1 255.255.255.224
 ip helper-address 172.16.3.248 
 ip ospf 110 area 1
!
interface Vlan3
 no shutdown
 ip address 192.168.3.33 255.255.255.224
 ip helper-address 172.16.3.248 
 ip ospf 110 area 1
!
router ospf 110
 router-id 4.4.4.4
 area 1 stub
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
ntp server 172.16.3.248

```

[Назад](readme.md)