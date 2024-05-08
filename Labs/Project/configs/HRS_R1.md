```

!
! No configuration change since last restart
! NVRAM config last updated at 08:03:53 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname HRS_R1
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
ip dhcp excluded-address 192.168.5.65
ip dhcp excluded-address 192.168.5.129
!
ip dhcp pool Sklad
 network 192.168.5.64 255.255.255.224
 default-router 192.168.5.65 
 dns-server 192.168.5.65 
!
ip dhcp pool SB
 network 192.168.5.128 255.255.255.224
 default-router 192.168.5.129 
 dns-server 192.168.5.129 
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
interface Tunnel1
 no shutdown
 ip address 10.200.0.5 255.255.255.224
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
 description To_K-Telecom_R1
 ip address 46.130.0.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no shutdown
 no ip address
!
interface Ethernet0/1.4
 no shutdown
 description Gateway_VLAN4
 encapsulation dot1Q 4
 ip address 192.168.5.65 255.255.255.224
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1.6
 no shutdown
 description Gateway_VLAN6
 encapsulation dot1Q 6
 ip address 192.168.5.129 255.255.255.224
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1.333
 no shutdown
 description MNG
 encapsulation dot1Q 333
 ip address 172.16.5.1 255.255.255.248
!
interface Ethernet0/1.777
 no shutdown
 encapsulation dot1Q 777 native
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
router rip
 version 2
 redistribute connected route-map RC
 network 10.0.0.0
 no auto-summary
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 46.130.0.1
!
ip access-list standard NAT_Hosts
 permit 192.168.5.0 0.0.0.255
 deny   any
!
!
ip prefix-list RC seq 1 deny 46.130.0.0/30
ip prefix-list RC seq 5 permit 192.168.5.64/27
ip prefix-list RC seq 10 permit 192.168.5.128/27
ip prefix-list RC seq 15 permit 172.16.5.0/29
!
route-map RC permit 5
 match ip address prefix-list RC
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
ntp master 1
ntp update-calendar
!
end

```

[Назад](readme.md)