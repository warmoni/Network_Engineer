## Базовая настройка коммутатора SW3

```
Current configuration : 1691 bytes
!
! Last configuration change at 08:55:18 UTC Mon Oct 16 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$3Dcq$sDfedOOpxyzruvnCCUdQe1
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
no ip domain-lookup
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
 shutdown
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 shutdown
!
interface Ethernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/0
 shutdown
!
interface Ethernet1/1
 shutdown
!
interface Ethernet1/2
 shutdown
!
interface Ethernet1/3
 shutdown
!
interface Vlan1
 ip address 192.168.1.3 255.255.255.0
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
banner login ^CC

$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$          All activity is subject to monitoring.         $$
$$      Any UNAUTHORIZED access or use is PROHIBITED,      $$
$$              and may result in PROSECUTION.             $$
$$                      << SW3 >>                          $$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

^C
!
line con 0
 password 7 05080F1C2243
 logging synchronous
line aux 0
line vty 0 4
 password 7 05080F1C2243
 login
 transport input telnet
!
!
end
```