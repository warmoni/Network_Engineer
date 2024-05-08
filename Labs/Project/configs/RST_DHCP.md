```

!
! No configuration change since last restart
! NVRAM config last updated at 08:02:35 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname RST_DHCP
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
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.33
ip dhcp excluded-address 192.168.1.65
ip dhcp excluded-address 192.168.1.97
ip dhcp excluded-address 192.168.1.129
ip dhcp excluded-address 192.168.1.161
!
ip dhcp pool Administration
 network 192.168.1.0 255.255.255.224
 default-router 192.168.1.1 
 dns-server 192.168.1.1 
!
ip dhcp pool Buhgalteria
 network 192.168.1.32 255.255.255.224
 default-router 192.168.1.33 
 dns-server 192.168.1.33 
!
ip dhcp pool Sklad
 network 192.168.1.64 255.255.255.224
 default-router 192.168.1.65 
 dns-server 192.168.1.65 
!
ip dhcp pool ATP
 network 192.168.1.96 255.255.255.224
 default-router 192.168.1.97 
 dns-server 192.168.1.97 
!
ip dhcp pool SB
 network 192.168.1.128 255.255.255.224
 default-router 192.168.1.129 
 dns-server 192.168.1.129 
!
ip dhcp pool IT
 network 192.168.1.160 255.255.255.224
 default-router 192.168.1.161 
 dns-server 192.168.1.161 
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
 ip address 172.16.1.247 255.255.255.255
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description To-RST_R1
 ip address 172.16.1.6 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/1
 no shutdown
 description To-RST_R2
 ip address 172.16.1.54 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 description To-RST_DSW1
 ip address 172.16.1.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/3
 no shutdown
 description To-RST_DSW2
 ip address 172.16.1.13 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
router ospf 110
 router-id 8.8.8.8
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