```
!
! No configuration change since last restart
! NVRAM config last updated at 08:03:09 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname RST_R1
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
 ip address 172.16.1.254 255.255.255.255
 ip ospf 110 area 0
!
interface Loopback2
 no shutdown
 no ip address
!
interface Ethernet0/0
 no shutdown
 description To-TTK_R1
 ip address 1.5.51.1 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no shutdown
 description To-RST_DSW1
 ip address 172.16.1.9 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 description To-RST_DSW2
 ip address 172.16.1.5 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/3
 no shutdown
 description To-RST_R2
 ip address 172.16.1.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet1/0
 no shutdown
 no ip address
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
 router-id 1.1.1.1
 area 2 stub no-summary
 default-information originate
!
router bgp 1551
 bgp log-neighbor-changes
 neighbor 1.5.51.2 remote-as 15774
 neighbor 172.16.1.253 remote-as 1551
 neighbor 172.16.1.253 update-source Loopback1
 !
 address-family ipv4
  network 1.5.51.0 mask 255.255.255.0
  neighbor 1.5.51.2 activate
  neighbor 1.5.51.2 soft-reconfiguration inbound
  neighbor 1.5.51.2 route-map To-TTK_R1 out
  neighbor 1.5.51.2 filter-list 1 out
  neighbor 172.16.1.253 activate
  neighbor 172.16.1.253 next-hop-self
  neighbor 172.16.1.253 soft-reconfiguration inbound
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 Null0
ip route 1.5.51.0 255.255.255.0 Null0
!
ip access-list standard NAT_Hosts
 permit 192.168.1.0 0.0.0.255
 deny   any
!
!
route-map To-TTK_R1 permit 5
 set as-path prepend 1551 1551 1551
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