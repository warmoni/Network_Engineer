```

!
! Last configuration change at 04:41:57 UTC Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname T-com_R1
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
 ip address 31.133.48.1 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 description To_RTK_R2
 ip address 79.133.64.10 255.255.255.252
!
interface Ethernet0/2
 no shutdown
 description To_UT-com_R1
 ip address 176.96.184.2 255.255.255.252
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
router bgp 22279
 bgp log-neighbor-changes
 neighbor 79.133.64.9 remote-as 12389
 neighbor 176.96.184.1 remote-as 26810
 !
 address-family ipv4
  network 31.133.48.0 mask 255.255.248.0
  neighbor 79.133.64.9 activate
  neighbor 79.133.64.9 soft-reconfiguration inbound
  neighbor 176.96.184.1 activate
  neighbor 176.96.184.1 soft-reconfiguration inbound
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 31.133.48.0 255.255.255.248 Null0
ip route 31.133.48.12 255.255.255.252 31.133.48.2
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