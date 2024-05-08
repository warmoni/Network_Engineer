```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname K-Telecom_R1
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
 no ip address
!
interface Ethernet0/0.333
 no shutdown
 description To_MSK-IX
 encapsulation dot1Q 333
 ip address 195.208.24.6 255.255.248.0
!
interface Ethernet0/1
 no shutdown
 description To_HRS_R1
 ip address 46.130.0.1 255.255.255.252
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
router bgp 43733
 no bgp enforce-first-as
 bgp log-neighbor-changes
 neighbor 195.208.24.1 remote-as 8985
 !
 address-family ipv4
  network 46.130.0.0 mask 255.255.255.0
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
ip route 46.130.0.0 255.255.255.0 Null0
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