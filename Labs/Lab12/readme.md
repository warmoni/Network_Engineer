# Основные протоколы сети Интернет

----

## Задание:

* Настройте NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
* Настройте NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
* Настройте статический NAT для R20.
* Настройте NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
* Настройте статический NAT(PAT) для офиса Чокурдах.
* Настройте для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
* Настройте NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
* Все офисы в лабораторной работе должны иметь IP связность.




### Схема реализации (EVE-NG)
\
![BigNet.png](img%2FBigNet.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).


### Настройка NAT(PAT) на R14 и R15

Для трансляции сетей офиса Москва использован NAT с перегрузкой (PAT). В качестве 
ресурса для преобразования адресов выступают внешние интерфейсы e0/2 на R14 и R15.
Чтоб указать сети, которые нужно транслировать, создан ACL NAT_Hosts. Интерфейсы e0/2
R14, R15 настроены как ip nat outside, остальные ip nat inside.

R14,R15 - настройки идентичны:
```
ip nat inside source list NAT_Hosts interface Ethernet0/2 overload
!
ip access-list standard NAT_Hosts
 permit 10.0.2.0 0.0.0.255
 permit 10.0.3.0 0.0.0.255
 deny   any
```

При работоспособности обоих аплинков (на R14 и R15) трафик будет выходить в мир
через приоритетный маршрут (с R15). Таким образом натить будет только R15, так как 
на R14 трафик через outside-интерфейс e0/2 идти не будет. В случае обрыва BGP-сессии
между Москвой и Ламасом, внешний мир будет доступен с R14, натить будет R14.

Просмотр NAT-трансляций на R15:
```
R15#sh ip nat tra
Pro Inside global      Inside local       Outside local      Outside global
icmp 100.1.0.5:54516   10.0.2.130:54516   20.42.0.1:54516    20.42.0.1:54516
icmp 100.1.0.5:54772   10.0.2.130:54772   20.42.0.1:54772    20.42.0.1:54772
icmp 100.1.0.5:55028   10.0.2.130:55028   20.42.0.1:55028    20.42.0.1:55028
icmp 100.1.0.5:55284   10.0.2.130:55284   20.42.0.1:55284    20.42.0.1:55284
icmp 100.1.0.5:55540   10.0.2.130:55540   20.42.0.1:55540    20.42.0.1:55540
icmp 100.1.0.5:58612   10.0.3.5:58612     52.0.0.1:58612     52.0.0.1:58612
icmp 100.1.0.5:59124   10.0.3.5:59124     52.0.0.1:59124     52.0.0.1:59124
icmp 100.1.0.5:59380   10.0.3.5:59380     52.0.0.1:59380     52.0.0.1:59380
icmp 100.1.0.5:59636   10.0.3.5:59636     52.0.0.1:59636     52.0.0.1:59636
icmp 100.1.0.5:59892   10.0.3.5:59892     52.0.0.1:59892     52.0.0.1:59892
```


### Настройка NAT(PAT) на R18

Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042 C.-Петербург.
Для этого на R18 создан пул NAT_Overload из 5 белых ip-адресов. Для анонса
адресов из этого пула прописан маршрут на Null 0. Сети для трансляции указаны в
ACL NAT_Hosts. Интерфейсы R18 e0/2-3 - outside, e0/0-1 - inside.

R18:
```
ip nat pool NAT_Overload 20.42.0.9 20.42.0.13 netmask 255.255.255.248
ip nat inside source list NAT_Hosts pool NAT_Overload overload
!
ip route 20.42.0.8 255.255.255.248 Null0
!
ip access-list standard NAT_Hosts
 permit 10.0.9.0 0.0.0.255
 permit 10.0.10.0 0.0.0.255
 deny   any
```


Просмотр NAT-трансляций на R18:
```
R18#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 20.42.0.10:31480  10.0.9.2:31480     100.1.0.1:31480    100.1.0.1:31480
icmp 20.42.0.10:31736  10.0.9.2:31736     100.1.0.1:31736    100.1.0.1:31736
icmp 20.42.0.10:31992  10.0.9.2:31992     100.1.0.1:31992    100.1.0.1:31992
icmp 20.42.0.10:32248  10.0.9.2:32248     100.1.0.1:32248    100.1.0.1:32248
icmp 20.42.0.10:32504  10.0.9.2:32504     100.1.0.1:32504    100.1.0.1:32504
icmp 20.42.0.10:35320  10.0.10.2:35320    30.1.0.1:35320     30.1.0.1:35320
icmp 20.42.0.10:35576  10.0.10.2:35576    30.1.0.1:35576     30.1.0.1:35576
icmp 20.42.0.10:35832  10.0.10.2:35832    30.1.0.1:35832     30.1.0.1:35832
icmp 20.42.0.10:36088  10.0.10.2:36088    30.1.0.1:36088     30.1.0.1:36088
icmp 20.42.0.10:36344  10.0.10.2:36344    30.1.0.1:36344     30.1.0.1:36344
```

### Настройка статического NAT для R20

На маршрутизаторах R14 и R15 добавлена запись для организации статического NAT для роутера
R20. Использован адрес 100.1.0.9 и роут на Null 0 (R15) для анонса в BGP.

R15:
```
ip nat inside source static 192.168.254.20 100.1.0.9
ip route 100.1.0.9 255.255.255.255 Null0
```


R14:
```
ip nat inside source static 192.168.254.20 100.1.0.9
```

Просмотр NAT-трансляций на R15
```
R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 100.1.0.9:2       192.168.254.20:2   20.42.0.1:2        20.42.0.1:2
icmp 100.1.0.9:3       192.168.254.20:3   20.42.0.5:3        20.42.0.5:3
--- 100.1.0.9          192.168.254.20     ---                ---
```


### Настройка NAT для удалённого управления R19

Настройка аналогична статического NAT для R20, за исключением указания протокола
и портов (внутренний/внешний). Адрес для трансляции - 100.1.0.8, внутренний порт 22,
внешний 222, протокол TCP. Маршрут на Null 0 для анонса в BGP прописан на R14.
В качестве протокола для удалённого управления выбран SSH. На R19 сгенерирован ключ RSA, 
создан пользователь admin, в качестве протокола для подключения указан SSH.


R15
```
ip nat inside source static tcp 192.168.254.19 22 100.1.0.8 222 extendable
```

R14:
```
ip nat inside source static tcp 192.168.254.19 22 100.1.0.8 222 extendable
ip route 100.1.0.8 255.255.255.255 Null0
```

R19:
```
username admin privilege 15 secret 5 $1$Z0w7$OVlAVS3PIv.pFBQqyqTQw1
!
ip ssh version 2
!
line vty 0 4
 login local
 transport input ssh
```

Проверка доступа из мира на R19 и NAT-трансляций на R15:
```
R18#ssh -l admin -p 222 100.1.0.8
Password:
R19#

R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
tcp 100.1.0.8:222      192.168.254.19:22  20.42.0.1:25502    20.42.0.1:25502
tcp 100.1.0.8:222      192.168.254.19:22  ---                ---
```


### Настройка статического NAT(PAT) для офиса Чокурдах

Офис Чокурдах не имеет собственной автономной системы. Для организации NAT, с Триады
смаршрутизирована сеть 52.0.0.16/28 с роутеров R25 и R26. Для резервирования
использованы SLA и Track. Ротуеры R25 и R26 анонсируют 52.0.0.16/28 по BGP.

R25:
```
track 1 ip sla 1 reachability
 delay down 10 up 10
!
ip route 52.0.0.16 255.255.255.240 52.0.0.6 50 track 1
!
ip sla 1
 icmp-echo 52.0.0.6 source-ip 52.0.0.5
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 1 life forever start-time now
```

R26:
```
track 1 ip sla 1 reachability
 delay down 10 up 10
ip route 52.0.0.16 255.255.255.240 52.0.0.10 track 1
!
ip sla 1
 icmp-echo 52.0.0.10 source-ip 52.0.0.9
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 1 life forever start-time now
```

R28:
```
ip nat inside source static network 10.0.29.0 52.0.0.16 /28
```


Проверка NAT-трансляций на R28:

```
R28#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 52.0.0.18:52993   10.0.29.2:52993    52.0.0.1:52993     52.0.0.1:52993
icmp 52.0.0.18:53249   10.0.29.2:53249    52.0.0.1:53249     52.0.0.1:53249
icmp 52.0.0.18:53505   10.0.29.2:53505    52.0.0.1:53505     52.0.0.1:53505
--- 52.0.0.18          10.0.29.2          ---                ---
--- 52.0.0.19          10.0.29.3          ---                ---
--- 52.0.0.16          10.0.29.0          ---                ---
```

### Настройка для IPv4 DHCP сервера в офисе Москва на маршрутизаторах R12 и R13

Ранее, в [лабораторной работе №4](..%2FLab04%2Freadme.md), произведена настройка 
DHCP-серверов на коммутаторах SW4 и SW5 для сетей 10.0.2.0/24, 10.0.3.0/24. Для переноса
настроек на R12 и R13 на VLAN-интерфейсах 2 и 3 коммутаторов SW4, SW5 прописан
ip helper-address 192.168.254.12, 192.168.254.13. На R12 и R13 натсроены соответствующие
DHCP-пулы.

SW4:
```
interface Vlan2
 description Gateway_VLAN_2
 ip address 10.0.2.3 255.255.255.0
 ip helper-address 192.168.254.12
 ip helper-address 192.168.254.13
 standby version 2
 standby 2 ipv6 FE80::10:0:2:1
 standby 2 ipv6 2001:DB8:1001:10:0:2:0:1/96
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:2:3 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 2 description Gateway_VLAN_2
 vrrp 2 ip 10.0.2.1
end
!
interface Vlan3
 description Gateway_VLAN_3
 ip address 10.0.3.2 255.255.255.0
 ip helper-address 192.168.254.12
 ip helper-address 192.168.254.13
 standby version 2
 standby 3 ipv6 FE80::10:0:3:1
 standby 3 ipv6 2001:DB8:1001:10:0:3:0:1/96
 standby 3 priority 101
 standby 3 preempt
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:3:2 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 3 description Gateway_VLAN_3
 vrrp 3 ip 10.0.3.1
 vrrp 3 priority 101
end
```

SW5:
```
interface Vlan2
 description Gateway_VLAN_2
 ip address 10.0.2.2 255.255.255.0
 ip helper-address 192.168.254.12
 ip helper-address 192.168.254.13
 standby version 2
 standby 2 ipv6 FE80::10:0:2:1
 standby 2 ipv6 2001:DB8:1001:10:0:2:0:1/96
 standby 2 priority 101
 standby 2 preempt
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:2:2 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 2 description Gateway_VLAN_2
 vrrp 2 ip 10.0.2.1
 vrrp 2 priority 101
end
!
interface Vlan3
 description Gateway_VLAN_3
 ip address 10.0.3.3 255.255.255.0
 ip helper-address 192.168.254.12
 ip helper-address 192.168.254.13
 standby version 2
 standby 3 ipv6 FE80::10:0:3:1
 standby 3 ipv6 2001:DB8:1001:10:0:3:0:1/96
 ip ospf network broadcast
 ip ospf 110 area 10
 ipv6 address FE80::10:0:3:3 link-local
 ipv6 enable
 ipv6 ospf 110 area 10
 ipv6 ospf network broadcast
 vrrp 3 description Gateway_VLAN_3
 vrrp 3 ip 10.0.3.1
end
```

R12:
```
ip dhcp excluded-address 10.0.3.1 10.0.3.3
ip dhcp excluded-address 10.0.2.1 10.0.2.128
ip dhcp excluded-address 10.0.3.128 10.0.3.254
!
ip dhcp pool HOSTS_VLAN_2
 network 10.0.2.0 255.255.255.0
 default-router 10.0.2.1
 dns-server 1.1.1.1 77.88.8.8
!
ip dhcp pool HOSTS_VLAN_3
 network 10.0.3.0 255.255.255.0
 dns-server 1.1.1.1 77.88.8.8
 default-router 10.0.3.1
```


R13:
```
ip dhcp excluded-address 10.0.2.1 10.0.2.3
ip dhcp excluded-address 10.0.2.128 10.0.2.254
ip dhcp excluded-address 10.0.3.1 10.0.3.128
!
ip dhcp pool DHCP_VLAN_2
 network 10.0.2.0 255.255.255.0
 default-router 10.0.2.1
 dns-server 1.1.1.1 77.88.8.8
!
ip dhcp pool HOSTS_VLAN_3
 network 10.0.3.0 255.255.255.0
 dns-server 1.1.1.1 77.88.8.8
 default-router 10.0.3.1
```

Получение IP-адресов на VPC1 и VPC7:
```
VPC1> ip dhcp
DORA IP 10.0.3.5/24 GW 10.0.3.1

VPC1> ping 52.0.0.18

84 bytes from 52.0.0.18 icmp_seq=1 ttl=56 time=10.233 ms
84 bytes from 52.0.0.18 icmp_seq=2 ttl=56 time=8.854 ms
84 bytes from 52.0.0.18 icmp_seq=3 ttl=56 time=11.427 ms
84 bytes from 52.0.0.18 icmp_seq=4 ttl=56 time=9.616 ms
84 bytes from 52.0.0.18 icmp_seq=5 ttl=56 time=8.270 ms

VPC7> ip dhcp
DORA IP 10.0.2.130/24 GW 10.0.2.1

VPC7> ping 20.42.0.1

84 bytes from 20.42.0.1 icmp_seq=1 ttl=250 time=7.826 ms
84 bytes from 20.42.0.1 icmp_seq=2 ttl=250 time=6.486 ms
84 bytes from 20.42.0.1 icmp_seq=3 ttl=250 time=7.036 ms
84 bytes from 20.42.0.1 icmp_seq=4 ttl=250 time=9.462 ms
84 bytes from 20.42.0.1 icmp_seq=5 ttl=250 time=7.347 ms
```


### Настройте NTP сервер на R12 и R13 для офиса Москва.


Роутер R12, R13 настроены в качестве NTP-master, часовой слой (stratum) выбран 1. 
Остальные сетевые устройства выступают в качестве клиентов NTP.

Настройка NTP на R12, R13:
```
ntp master 1
ntp update-calendar
```

Настройка клиентов идентична:
```
ntp update-calendar
ntp server 192.168.254.12 prefer
ntp server 192.168.254.13
```

Проверка NTP на R15:
```
R15#sh NTP associations

  address         ref clock       st   when   poll reach  delay  offset   disp
*~192.168.254.12  .LOCL.           1    253   1024   377  1.000   0.500  1.974
+~192.168.254.13  .LOCL.           1    950   1024   377  1.000  -0.500  1.974
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

R15#sh ntp status
Clock is synchronized, stratum 2, reference is 192.168.254.12
nominal freq is 250.0000 Hz, actual freq is 250.0000 Hz, precision is 2**10
ntp uptime is 9618500 (1/100 of seconds), resolution is 4000
reference time is E98986F6.27AE14E8 (13:18:30.155 MSK Wed Feb 28 2024)
clock offset is 0.5000 msec, root delay is 1.00 msec
root dispersion is 9.65 msec, peer dispersion is 1.97 msec
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is 0.000000000 s/s
system poll interval is 1024, last update was 262 sec ago.

R15#show clock detail
13:22:58.952 MSK Wed Feb 28 2024
Time source is NTP
```

