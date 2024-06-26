```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname RTK_R1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
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
interface Ethernet0/0
 no shutdown
 description To_RTK_R2
 ip address 79.133.64.1 255.255.255.252
 ip router isis 
!
interface Ethernet0/1
 no shutdown
 description To_UT-com_R1
 ip address 79.133.64.5 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 no ip address
!
interface Ethernet0/2.33
 no shutdown
!
interface Ethernet0/2.333
 no shutdown
 description To_MSK-IX
 encapsulation dot1Q 333
 ip address 195.208.24.5 255.255.248.0
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
router isis
 net 49.0001.1238.9000.0001.00
 redistribute connected
!
router bgp 12389
 no bgp enforce-first-as
 bgp log-neighbor-changes
 neighbor 79.133.64.2 remote-as 12389
 neighbor 79.133.64.6 remote-as 26810
 neighbor 195.208.24.1 remote-as 8985
 !
 address-family ipv4
  network 79.133.64.0 mask 255.255.255.0
  neighbor 79.133.64.2 activate
  neighbor 79.133.64.2 next-hop-self
  neighbor 79.133.64.2 soft-reconfiguration inbound
  neighbor 79.133.64.6 activate
  neighbor 79.133.64.6 soft-reconfiguration inbound
  neighbor 195.208.24.1 activate
  neighbor 195.208.24.1 send-community both
  neighbor 195.208.24.1 soft-reconfiguration inbound
  neighbor 195.208.24.1 route-map RS_Community out
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 79.133.64.0 255.255.255.0 Null0
!
!
route-map RS_Community permit 10
 set community 588849945
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
!
end

```

[Назад](readme.md)