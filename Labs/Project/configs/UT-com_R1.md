```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname UT-com_R1
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
 description To_DPR_R1
 ip address 176.96.184.5 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description To_Megafon_R2
 ip address 85.26.242.10 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description To_T-com_R1
 ip address 176.96.184.1 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description To_RTK_R1
 ip address 79.133.64.6 255.255.255.252
!
interface Ethernet1/0
 no shutdown
 description To_L-com_R1
 ip address 176.96.184.9 255.255.255.252
!
interface Ethernet1/1
 no shutdown
 no ip address
!
interface Ethernet1/1.333
 no shutdown
 description To_MSK-IX
 encapsulation dot1Q 333
 ip address 195.208.24.4 255.255.248.0
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
router bgp 26810
 no bgp enforce-first-as
 bgp log-neighbor-changes
 neighbor 79.133.64.5 remote-as 12389
 neighbor 85.26.242.9 remote-as 31133
 neighbor 176.96.184.2 remote-as 22279
 neighbor 176.96.184.10 remote-as 4321
 neighbor 195.208.24.1 remote-as 8985
 !
 address-family ipv4
  network 31.133.56.0 mask 255.255.248.0
  network 176.96.184.0 mask 255.255.248.0
  neighbor 79.133.64.5 activate
  neighbor 79.133.64.5 soft-reconfiguration inbound
  neighbor 85.26.242.9 activate
  neighbor 85.26.242.9 soft-reconfiguration inbound
  neighbor 176.96.184.2 activate
  neighbor 176.96.184.2 soft-reconfiguration inbound
  neighbor 176.96.184.10 activate
  neighbor 176.96.184.10 soft-reconfiguration inbound
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
ip route 31.133.56.0 255.255.248.0 Null0
ip route 176.96.184.0 255.255.248.0 Null0
ip route 176.96.184.12 255.255.255.252 176.96.184.6
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