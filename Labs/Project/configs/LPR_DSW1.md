```

!
! Last configuration change at 14:51:00 MSK Wed May 8 2024
! NVRAM config last updated at 08:02:59 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname LPR_DSW1
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
ip dhcp excluded-address 192.168.2.1
ip dhcp excluded-address 192.168.2.33
ip dhcp excluded-address 192.168.2.65
ip dhcp excluded-address 192.168.2.129
!
ip dhcp pool Administration
 network 192.168.2.0 255.255.255.224
 default-router 192.168.2.1 
 dns-server 192.168.2.1 
!
ip dhcp pool Buhgalteria
 network 192.168.2.32 255.255.255.224
 default-router 192.168.2.33 
 dns-server 192.168.2.33 
!
ip dhcp pool Sklad
 network 192.168.2.64 255.255.255.224
 default-router 192.168.2.65 
 dns-server 192.168.2.65 
!
ip dhcp pool Okhrana
 network 192.168.2.128 255.255.255.224
 default-router 192.168.2.129 
 dns-server 192.168.2.129 
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
 ip address 172.16.2.253 255.255.255.255
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description To-LPR_R1
 no switchport
 ip address 172.16.2.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/1
 no shutdown
 description To-LPR_ASW1
 no switchport
 ip address 172.16.2.5 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/2
 no shutdown
 description To-LPR_ASW2
 no switchport
 ip address 172.16.2.9 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/3
 no shutdown
 description To-LPR_ASW3
 no switchport
 ip address 172.16.2.13 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet1/0
 no shutdown
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
ntp master 1
ntp update-calendar
!
end

```

[Назад](readme.md)