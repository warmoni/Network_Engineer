```

!
! Last configuration change at 14:53:17 MSK Wed May 8 2024
! NVRAM config last updated at 08:03:11 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname DPR_DHCP
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
clock timezone MSK 3 0
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!
!
!


!
ip dhcp excluded-address 192.168.3.1
ip dhcp excluded-address 192.168.3.33
ip dhcp excluded-address 192.168.3.65
ip dhcp excluded-address 192.168.3.129
!
ip dhcp pool Administration
 network 192.168.3.0 255.255.255.224
 default-router 192.168.3.1 
 dns-server 192.168.3.1 
!
ip dhcp pool Buhgalteria
 network 192.168.3.32 255.255.255.224
 default-router 192.168.3.33 
 dns-server 192.168.3.33 
!
ip dhcp pool Sklad
 network 192.168.3.64 255.255.255.224
 default-router 192.168.3.65 
 dns-server 192.168.3.65 
!
ip dhcp pool Okhrana
 network 192.168.3.128 255.255.255.224
 default-router 192.168.3.129 
 dns-server 192.168.3.129 
!
!
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
redundancy
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
 ip address 172.16.3.248 255.255.255.255
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description To-DPR_DSW1
 ip address 172.16.3.14 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/1
 no shutdown
 description To-DPR_DSW2
 ip address 172.16.3.18 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 description To-DPR_R1
 ip address 172.16.3.10 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/0
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/1
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/3
 no shutdown
 no ip address
 shutdown
!
router ospf 110
 router-id 7.7.7.7
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
ntp master 1
ntp update-calendar
!
end

```

[Назад](readme.md)