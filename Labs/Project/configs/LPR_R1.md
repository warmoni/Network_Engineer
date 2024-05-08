```

!
! No configuration change since last restart
! NVRAM config last updated at 08:03:16 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname LPR_R1
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
 description MNG
 ip address 172.16.2.254 255.255.255.255
 ip ospf 110 area 0
!
interface Tunnel1
 no shutdown
 ip address 10.200.0.3 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 no shutdown
 description To_Lugacom_R1
 ip address 5.42.216.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no shutdown
 description To-LPR_DSW1
 ip address 172.16.2.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
!
router ospf 110
 router-id 1.1.1.1
 default-information originate always
!
router rip
 version 2
 redistribute ospf 110 metric 2
 network 10.0.0.0
 no auto-summary
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 5.42.216.1
!
ip access-list standard NAT_Hosts
 permit 192.168.2.0 0.0.0.255
 deny   any
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
ntp update-calendar
ntp server 172.16.2.253
!
end

```

[Назад](readme.md)