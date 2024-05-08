```

!
! Last configuration change at 11:14:28 UTC Wed May 8 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname MSK-IX_SW
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
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
 description To_RS
 switchport access vlan 333
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet0/1
 no shutdown
 description To_RTK_R1
 switchport trunk allowed vlan 333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no lldp transmit
 no lldp receive
 no cdp enable
 spanning-tree bpduguard enable
!
interface Ethernet0/2
 no shutdown
 description To_Megafon_R1
 switchport trunk allowed vlan 333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no lldp transmit
 no lldp receive
 no cdp enable
 spanning-tree bpduguard enable
!
interface Ethernet0/3
 no shutdown
 description To_K-Telecom_R1
 switchport trunk allowed vlan 333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no lldp transmit
 no lldp receive
 no cdp enable
 spanning-tree bpduguard enable
!
interface Ethernet1/0
 no shutdown
 description To_TTK_R1
 switchport trunk allowed vlan 333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no lldp transmit
 no lldp receive
 no cdp enable
 spanning-tree bpduguard enable
!
interface Ethernet1/1
 no shutdown
 description To_Ugletelecom_R1
 switchport trunk allowed vlan 333
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no lldp transmit
 no lldp receive
 no cdp enable
 spanning-tree bpduguard enable
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
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
!
end

```

[Назад](readme.md)