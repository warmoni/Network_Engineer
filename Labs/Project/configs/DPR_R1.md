```

!
! No configuration change since last restart
! NVRAM config last updated at 08:03:38 MSK Fri May 10 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname DPR_R1
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
track 1 ip sla 1 reachability
 delay down 10 up 10
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
 description For_NAT
 ip address 31.133.48.13 255.255.255.252 secondary
 ip address 176.96.184.13 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Loopback1
 no shutdown
 description MNG
 ip address 172.16.3.254 255.255.255.255
 ip ospf 110 area 0
!
interface Tunnel1
 no shutdown
 ip address 10.200.0.4 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Loopback0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 no shutdown
 description To_Ugletelecom_R1
 ip address 176.96.184.6 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no shutdown
 description To-DPR_DSW1
 ip address 172.16.3.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/2
 no shutdown
 description To-DPR_DHCP
 ip address 172.16.3.9 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
!
interface Ethernet1/0
 no shutdown
 description To_Comtel_R1
 ip address 31.133.48.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet1/1
 no shutdown
 description To-DPR_DSW2
 ip address 172.16.3.5 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 110 area 0
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
 default-information originate always
!
router rip
 version 2
 redistribute ospf 110 metric 2
 network 10.0.0.0
 no auto-summary
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list NAT_Hosts interface Loopback0 overload
ip route 0.0.0.0 0.0.0.0 176.96.184.5 50 track 1
ip route 0.0.0.0 0.0.0.0 31.133.48.1 100
!
ip access-list standard NAT_Hosts
 permit 192.168.3.0 0.0.0.255
 deny   any
!
ip sla 1
 icmp-echo 176.96.184.5 source-ip 176.96.184.6
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 1 life forever start-time now
!
route-map Test permit 5
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
ntp server 172.16.3.248
!
end

```

[Назад](readme.md)