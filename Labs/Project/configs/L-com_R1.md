```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname L-com_R1
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
 description To_LPR_R1
 ip address 5.42.216.1 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description To_UT-com_R1
 ip address 176.96.184.10 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description To_Megafon_R1
 ip address 85.26.242.6 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
!
router bgp 4321
 bgp log-neighbor-changes
 neighbor 85.26.242.5 remote-as 31133
 neighbor 176.96.184.9 remote-as 26810
 !
 address-family ipv4
  network 5.42.216.0 mask 255.255.255.0
  neighbor 85.26.242.5 activate
  neighbor 85.26.242.5 soft-reconfiguration inbound
  neighbor 176.96.184.9 activate
  neighbor 176.96.184.9 soft-reconfiguration inbound
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 5.42.216.0 255.255.255.0 Null0
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
!
end

```

[Назад](readme.md)