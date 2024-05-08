```

!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Megafon_R2
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
 description To_Megafon_R1
 ip address 85.26.242.2 255.255.255.252
 ip router isis 
!
interface Ethernet0/1
 no shutdown
 description To_Ugletelecom_R1
 ip address 85.26.242.9 255.255.255.252
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
router isis
 net 49.0001.3113.3000.0002.00
 redistribute connected
!
router bgp 31133
 bgp log-neighbor-changes
 neighbor 85.26.242.1 remote-as 31133
 neighbor 85.26.242.10 remote-as 26810
 !
 address-family ipv4
  network 85.26.242.0 mask 255.255.255.0
  neighbor 85.26.242.1 activate
  neighbor 85.26.242.1 next-hop-self
  neighbor 85.26.242.1 soft-reconfiguration inbound
  neighbor 85.26.242.10 activate
  neighbor 85.26.242.10 soft-reconfiguration inbound
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 85.26.242.0 255.255.255.0 Null0
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