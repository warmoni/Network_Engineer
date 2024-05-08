```

!
! Last configuration change at 14:15:15 MSK Wed May 8 2024
! NVRAM config last updated at 08:03:35 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DPR_DSW1
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
 ip address 172.16.3.253 255.255.255.255
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description TO-DPR_R1
 no switchport
 ip address 172.16.3.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/1
 no shutdown
 description To-DPR_DHCP
 no switchport
 ip address 172.16.3.13 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/2
 no shutdown
 description To-DPR_ASW1
 no switchport
 ip address 172.16.3.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet0/3
 no shutdown
 description To-DPR_ASW2
 no switchport
 ip address 172.16.3.25 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet1/0
 no shutdown
 description To-DPR_ASW3
 no switchport
 ip address 172.16.3.29 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
router ospf 110
 router-id 2.2.2.2
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
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp update-calendar
ntp server 172.16.3.248
!
end

```

[Назад](readme.md)