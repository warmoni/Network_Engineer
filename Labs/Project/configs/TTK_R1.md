```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname TTK_R1
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
no ip domain lookup
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
 no ip address
!
interface Ethernet0/0.33
 no shutdown
!
interface Ethernet0/0.333
 no shutdown
 description To_MSK-IX
 encapsulation dot1Q 333
 ip address 195.208.24.2 255.255.248.0
!
interface Ethernet0/1
 no shutdown
 description To_RST_R1
 ip address 1.5.51.2 255.255.255.252
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
router bgp 15774
 no bgp enforce-first-as
 bgp log-neighbor-changes
 neighbor 1.5.51.1 remote-as 1551
 neighbor 195.208.24.1 remote-as 8985
 !
 address-family ipv4
  network 1.5.51.0 mask 255.255.255.0
  network 5.8.192.0 mask 255.255.252.0
  neighbor 1.5.51.1 activate
  neighbor 1.5.51.1 soft-reconfiguration inbound
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
ip route 5.8.192.0 255.255.252.0 Null0
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