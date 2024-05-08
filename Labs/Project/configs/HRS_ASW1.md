```

!
! Last configuration change at 07:15:04 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname HRS_ASW1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
clock timezone MSK 3 0
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
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
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
 description To-HRS_DSW1
 switchport trunk allowed vlan 4,333,777
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 no cdp enable
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 4
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet1/0
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet1/1
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet1/2
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet1/3
 no shutdown
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast edge
!
interface Vlan333
 no shutdown
 description MNG
 ip address 172.16.5.3 255.255.255.248
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp update-calendar
ntp server 172.16.5.1
!
end

```

[Назад](readme.md)