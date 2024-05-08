```
!
! Last configuration change at 14:15:57 MSK Wed May 8 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname RST_ASW1
!
boot-start-marker
boot-end-marker
!
!
logging console informational
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
ip cef load-sharing algorithm original
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
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
interface Loopback1
 no shutdown
 ip address 172.16.1.250 255.255.255.255
 ip ospf 110 area 1
!
interface GigabitEthernet0/0
 no shutdown
 description To-RST_DSW1
 no switchport
 ip address 172.16.1.30 255.255.255.252
 ip helper-address 172.16.1.6 
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/1
 no shutdown
 description To-RST_DSW2
 no switchport
 ip address 172.16.1.42 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex half
 no negotiation auto
!
interface GigabitEthernet0/2
 no shutdown
 switchport access vlan 2
 switchport mode access
 duplex half
 no negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet0/3
 no shutdown
 switchport access vlan 3
 switchport mode access
 duplex half
 no negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/0
 no shutdown
 switchport access vlan 2
 switchport mode access
 duplex half
 no negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/1
 no shutdown
 duplex half
 no negotiation auto
!
interface GigabitEthernet1/2
 no shutdown
 duplex half
 no negotiation auto
!
interface GigabitEthernet1/3
 no shutdown
 duplex half
 no negotiation auto
!
interface Vlan2
 no shutdown
 ip address 192.168.1.1 255.255.255.224
 ip helper-address 172.16.1.247 
 ip ospf network point-to-point
 ip ospf 110 area 1
!
interface Vlan3
 no shutdown
 ip address 192.168.1.33 255.255.255.224
 ip helper-address 172.16.1.247 
 ip ospf network point-to-point
 ip ospf 110 area 1
!
router ospf 110
 router-id 5.5.5.5
 area 1 stub
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
!
!
!
!
control-plane
!
banner exec ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
banner incoming ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
banner login ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp server 172.16.1.6

```

[Назад](readme.md)