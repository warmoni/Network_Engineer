## Базовая настройка коммутатора SW2

```
Current configuration : 1686 bytes
!
! Last configuration change at 08:52:36 UTC Mon Oct 16 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW2
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$TPH.$FUeI6Sxeql9X7vLiH4vGJ0
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
 ip address 192.168.1.2 255.255.255.0
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
banner login ^C
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$          All activity is subject to monitoring.         $$
$$      Any UNAUTHORIZED access or use is PROHIBITED,      $$
$$              and may result in PROSECUTION.             $$
$$                      << SW2 >>                          $$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
^C
!
line con 0
 password 7 01100F175804
 logging synchronous
line aux 0
line vty 0 4
 password 7 121A0C041104
 login
 transport input telnet
!
!
end
```

[Назад](readme.md)