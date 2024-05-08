```

!
! No configuration change since last restart
! NVRAM config last updated at 08:03:07 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname RST_R2
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
interface Loopback0
 no shutdown
 ip address 1.5.51.254 255.255.255.255
!
interface Loopback1
 no shutdown
 description MNG
 ip address 172.16.1.253 255.255.255.255
 ip ospf 110 area 0
!
interface Tunnel1
 no shutdown
 ip address 10.200.0.1 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map multicast dynamic
 ip nhrp network-id 200
 no ip split-horizon
 ip tcp adjust-mss 1400
 tunnel source Loopback0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 no shutdown
 description To_Megafon_R1
 ip address 1.5.51.5 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no shutdown
 description To-RST_DSW2
 ip address 172.16.1.17 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 description To-RST_DHCP
 ip address 172.16.1.53 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/3
 no shutdown
 description To-RST_R1
 ip address 172.16.1.2 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
router ospf 110
 router-id 2.2.2.2
 redistribute rip metric 1 subnets
!
router rip
 version 2
 redistribute ospf 110 metric 5
 network 10.0.0.0
 no auto-summary
!
router bgp 1551
 bgp log-neighbor-changes
 neighbor 1.5.51.6 remote-as 31133
 neighbor 172.16.1.254 remote-as 1551
 neighbor 172.16.1.254 update-source Loopback1
 !
 address-family ipv4
  network 1.5.51.0 mask 255.255.255.0
  neighbor 1.5.51.6 activate
  neighbor 1.5.51.6 soft-reconfiguration inbound
  neighbor 1.5.51.6 route-map To-Megafon_R1 in
  neighbor 1.5.51.6 filter-list 1 out
  neighbor 172.16.1.254 activate
  neighbor 172.16.1.254 next-hop-self
  neighbor 172.16.1.254 soft-reconfiguration inbound
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
ip route 1.5.51.0 255.255.255.0 Null0
!
ip access-list standard NAT_Hosts
 permit 192.168.1.0 0.0.0.255
 deny   any
!
!
route-map To-Megafon_R1 permit 5
 set local-preference 150
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
ntp server 172.16.1.6
!
end

```

[Назад](readme.md)