# iBGP

----

## Задание:

* Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.
* Настроите iBGP в провайдере Триада, с использованием RR.
* Настройте офис Москва так, чтобы приоритетным провайдером стал Ламас.
* Настройте офис С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
* Все сети в лабораторной работе должны иметь IP связность.


### Схема реализации (EVE-NG)
\
![BigNet.png](img%2FBigNet.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).

### План адресации IPv4/IPv6



|                |           |               |                |     AS1001      |                                |               |                      |               |
|:---------------|:----------|:--------------|:--------------:|:---------------:|:------------------------------:|:-------------:|:--------------------:|:-------------:|
|                |           |               |                |     Москва      |                                |               |                      |               |
| Hostname       | Interface | Description   |  IPv4-address  |      Mask       |         IPv6 GUA / ULA         | Prefix length |       IPv6 LLA       | Prefix length |
| R14            | e0/0      | To_R12        |  172.16.1.13   |       /30       | FD00:3CD5:1001:172:16:1:12:13  |     /112      |  fe80::172:16:1:13   | /10           |
|                | e0/1      | To_R13        |  172.16.1.17   |       /30       | FD00:3CD5:1001:172:16:1:16:17  |     /112      |  fe80::172:16:1:17   | /10           |
|                | e0/2      | To_R22_AS101  |   100.1.0.1    |       /30       |     2001:DB8:1001:100:1::1     |     /112      |   fe80::100:1:0:1    | /10           |
|                | e0/3      | To_R19        |  172.16.1.21   |       /30       | FD00:3CD5:1001:172:16:1:20:21  |     /112      |  fe80::172:16:1:21   | /10           |
|                | e1/0      | To_R15        |  172.16.1.45   |       /30       | FD00:3CD5:1001:172:16:1:44:45  |     /112      |  fe80::172:16:1:45   | /10           |
|                | Lo1       | Lo1           | 192.168.254.14 |       /32       | FD00:3CD5:1001::192:168:254:14 |     /128      | fe80::192:168:254:14 | /10           |
| R15            | e0/0      | To_R13        |   172.16.1.1   |       /30       |   FD00:3CD5:1001:172:16:1::1   |     /112      |   fe80::172:16:1:1   | /10           |
|                | e0/1      | To_R12        |   172.16.1.5   |       /30       |  FD00:3CD5:1001:172:16:1:4:5   |     /112      |   fe80::172:16:1:5   | /10           |
|                | e0/2      | To_R21_AS301  |   100.1.0.5    |       /30       |    2001:DB8:1001:100:1::4:5    |     /112      |   fe80::100:1:0:5    | /10           |
|                | e0/3      | To_R20        |   172.16.1.9   |       /30       |  FD00:3CD5:1001:172:16:1:8:9   |     /112      |   fe80::172:16:1:9   | /10           |
|                | e1/0      | To_R14        |  172.16.1.46   |       /30       | FD00:3CD5:1001:172:16:1:44:46  |     /112      |  fe80::172:16:1:46   | /10           |
|                | Lo1       | Lo1           | 192.168.254.15 |       /32       | FD00:3CD5:1001::192:168:254:15 |     /128      | fe80::192:168:254:15 | /10           |
|                |           |                  |                |      AS101      |                                |               |                      |               |
|                |           |                  |                |     Киторн      |                                |               |                      |               |
| Hostname       | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R22            | e0/0      | To_R14_AS1001    |   100.1.0.2    |       /30       |     2001:DB8:1001:100:1::2     |     /112      |   fe80::100:1:0:2    | /10           |
|                | e0/1      | To_R21_AS301     |   101.0.0.5    |       /30       |     2001:DB8:101:101::4:5      |     /112      |   fe80::101:0:0:5    | /10           |
|                | e0/2      | To_R23_AS520     |   101.0.0.1    |       /30       |      2001:DB8:101:101::1       |     /112      |   fe80::101:0:0:1    | /10           |
|                | Lo1       | Lo1              | 192.168.254.22 |       /32       | FD00:3CD5:101::192:168:254:22  |     /128      | fe80::192:168:254:22 | /10           |
|                |           |                  |                |      AS301      |                                |               |                      |               |
|                |           |                  |                |      Ламас      |                                |               |                      |               |
| Hostname       | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R21            | e0/0      | To_R15_AS1001    |   100.1.0.6    |       /30       |    2001:DB8:1001:100:1::4:6    |     /112      |   fe80::100:1:0:6    | /10           |
|                | e0/1      | To_R22_AS101     |   101.0.0.6    |       /30       |     2001:DB8:101:101::4:6      |     /112      |   fe80::101:0:0:6    | /10           |
|                | e0/2      | To_R24_AS520     |    30.1.0.1    |       /30       |      2001:DB8:301:30:1::1      |     /112      |    fe80::30:1:0:1    | /10           |
|                | Lo1       | Lo1              | 192.168.254.21 |       /32       | FD00:3CD5:301::192:168:254:21  |     /128      | fe80::192:168:254:21 | /10           |
|                |           |                  |                |      AS520      |                                |               |                      |               |
|                |           |                  |                |     Триада      |                                |               |                      |               |
| Hostname       | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R23            | e0/0      | To_R22_AS101     |   101.0.0.2    |       /30       |      2001:DB8:101:101::2       |     /112      |   fe80::101:0:0:2    | /10           |
|                | e0/1      | To_R25           |   172.16.2.1   |       /30       |   FD00:3CD5:520:172:16:2::1    |     /112      |   fe80::172:16:2:1   | /10           |
|                | e0/2      | To_R24           |   172.16.2.5   |       /30       |   FD00:3CD5:520:172:16:2:4:5   |     /112      |   fe80::172:16:2:5   | /10           |
|                | Lo1       | Lo1              | 192.168.254.23 |       /32       | FD00:3CD5:520::192:168:254:23  |     /128      | fe80::192:168:254:23 | /10           |
| R24            | e0/0      | To_R21_AS301     |    30.1.0.2    |       /30       |      2001:DB8:301:30:1::2      |     /112      |    fe80::30:1:0:2    | /10           |
|                | e0/1      | To_R26           |   172.16.2.9   |       /30       |   FD00:3CD5:520:172:16:2:8:9   |     /112      |   fe80::172:16:2:9   | /10           |
|                | e0/2      | To_R23           |   172.16.2.6   |       /30       |   FD00:3CD5:520:172:16:2:4:6   |     /112      |   fe80::172:16:2:6   | /10           |
|                | e0/3      | To_R18_AS2042    |   20.42.0.2    |       /30       |     2001:DB8:2042:20:42::2     |     /112      |   fe80::20:42:0:2    | /10           |
|                | Lo1       | Lo1              | 192.168.254.24 |       /32       | FD00:3CD5:520::192:168:254:24  |     /128      | fe80::192:168:254:24 | /10           |
| R25            | e0/0      | To_R23           |   172.16.2.2   |       /30       |   FD00:3CD5:520:172:16:2::2    |     /112      |   fe80::172:16:2:2   | /10           |
|                | e0/1      | To_R27_AS1001    |    52.0.0.1    |       /30       |       2001:DB8:520:52::1       |     /112      |    fe80::52:0:0:1    | /10           |
|                | e0/2      | To_R26           |  172.16.2.14   |       /30       |  FD00:3CD5:520:172:16:2:12:14  |     /112      |  fe80::172:16:2:14   | /10           |
|                | e0/3      | To_R28_AS1001    |    52.0.0.5    |       /30       |      2001:DB8:520:52::4:5      |     /112      |    fe80::52:0:0:5    | /10           |
|                | Lo1       | Lo1              | 192.168.254.25 |       /32       | FD00:3CD5:520::192:168:254:25  |     /128      | fe80::192:168:254:25 | /10           |
| R26            | e0/0      | To_R24           |  172.16.2.10   |       /30       |  FD00:3CD5:520:172:16:2:8:10   |     /112      |  fe80::172:16:2:10   | /10           |
|                | e0/1      | To_R28_AS1001    |    52.0.0.9    |       /30       |      2001:DB8:520:52::8:9      |     /112      |    fe80::52:0:0:9    | /10           |
|                | e0/2      | To_R25           |  172.16.2.13   |       /30       |  FD00:3CD5:520:172:16:2:12:13  |     /112      |  fe80::172:16:2:13   | /10           |
|                | e0/3      | To_R18_AS2042    |   20.42.0.6    |       /30       |    2001:DB8:2042:20:42::4:6    |     /112      |   fe80::20:42:0:6    | /10           |
|                | Lo1       | Lo1              | 192.168.254.26 |       /32       | FD00:3CD5:520::192:168:254:26  |     /128      | fe80::192:168:254:26 | /10           |
|                |           |                  |                |     AS2042      |                                |               |                      |               |
|                |           |                  |                | Санкт-Петербург |                                |               |                      |               |
| Hostname       | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R18            | e0/2      | To_R24_AS520     |   20.42.0.1    |       /30       |     2001:DB8:2042:20:42::1     |     /112      |   fe80::20:42:0:1    | /10           |
|                | e0/3      | To_R26_AS520     |   20.42.0.5    |       /30       |    2001:DB8:2042:20:42::4:5    |     /112      |   fe80::20:42:0:5    | /10           |
|                | Lo1       | Lo1              | 192.168.254.18 |       /32       | FD00:3CD5:2042::192:168:254:18 |     /128      | fe80::192:168:254:18 | /10           |


### Настройка iBGP в офисе Москва

Для резервирования доступа в мир принято решение анонсить default в OSPF
с двух пограничных роутеров R14 и R15. Также при установке приоритетного пути через Ламас выявлена
ситуация, когда ICMP-пакет с роуера R14 в сторону другой AS начинает петлять в домене OSPF, так как 
для R12 и R13 эта сеть неизвестна и он отправляет пакет на default (либо R14, либо R15).
В связи с этим принято решение в офисе Москва организовать связность по iBGP роутеров
R15, R14, R13, R12. Роутер R15 сделать RR, остальные - его клиенты. Таким образом ситуация 
с петлёй маршрутизации исключается, а резервирование остаётся даже в случает отказа R15 - через OSPF
и default с R14. Для оптимизации прохождения трафика от R14 до R15 добавлен физический линк между ними.
Для приоритезации маршрута через Ламас использоовались атрибуты Local Preference на R15
и AS-Path Prepend на R14.

R14:
```
R14#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 neighbor 2001:DB8:1001:100:1::2 remote-as 101
 neighbor 100.1.0.2 remote-as 101
 neighbor 192.168.254.15 remote-as 1001
 neighbor 192.168.254.15 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:15 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:15 update-source Loopback1
 !
 address-family ipv4
  network 100.1.0.0 mask 255.255.255.252
  network 192.168.254.14 mask 255.255.255.255
  no neighbor 2001:DB8:1001:100:1::2 activate
  neighbor 100.1.0.2 activate
  neighbor 100.1.0.2 soft-reconfiguration inbound
  neighbor 100.1.0.2 route-map PP_Kitorn out
  neighbor 192.168.254.15 activate
  neighbor 192.168.254.15 soft-reconfiguration inbound
  no neighbor FD00:3CD5:1001:0:192:168:254:15 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:1001:100:1::/112
  network FD00:3CD5:1001:0:192:168:254:14/128
  neighbor 2001:DB8:1001:100:1::2 activate
  neighbor 2001:DB8:1001:100:1::2 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1::2 route-map PP_Kitorn out
  neighbor FD00:3CD5:1001:0:192:168:254:15 activate
  neighbor FD00:3CD5:1001:0:192:168:254:15 soft-reconfiguration i                                                                       nbound
 exit-address-family
```

R15:
```
R15#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 neighbor 2001:DB8:1001:100:1:0:4:6 remote-as 301
 neighbor 100.1.0.6 remote-as 301
 neighbor 192.168.254.12 remote-as 1001
 neighbor 192.168.254.12 update-source Loopback1
 neighbor 192.168.254.13 remote-as 1001
 neighbor 192.168.254.13 update-source Loopback1
 neighbor 192.168.254.14 remote-as 1001
 neighbor 192.168.254.14 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:12 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:12 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:13 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:13 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:14 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:14 update-source Loopback1
 !
 address-family ipv4
  network 100.1.0.4 mask 255.255.255.252
  network 192.168.254.15 mask 255.255.255.255
  no neighbor 2001:DB8:1001:100:1:0:4:6 activate
  neighbor 100.1.0.6 activate
  neighbor 100.1.0.6 soft-reconfiguration inbound
  neighbor 100.1.0.6 route-map LP_Lamas in
  neighbor 192.168.254.12 activate
  neighbor 192.168.254.12 route-reflector-client
  neighbor 192.168.254.12 soft-reconfiguration inbound
  neighbor 192.168.254.13 activate
  neighbor 192.168.254.13 route-reflector-client
  neighbor 192.168.254.13 soft-reconfiguration inbound
  neighbor 192.168.254.14 activate
  neighbor 192.168.254.14 route-reflector-client
  neighbor 192.168.254.14 soft-reconfiguration inbound
  no neighbor FD00:3CD5:1001:0:192:168:254:12 activate
  no neighbor FD00:3CD5:1001:0:192:168:254:13 activate
  no neighbor FD00:3CD5:1001:0:192:168:254:14 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:1001:100:1:0:4:0/112
  network FD00:3CD5:1001:0:192:168:254:15/128
  neighbor 2001:DB8:1001:100:1:0:4:6 activate
  neighbor 2001:DB8:1001:100:1:0:4:6 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1:0:4:6 route-map LP_Lamas in
  neighbor FD00:3CD5:1001:0:192:168:254:12 activate
  neighbor FD00:3CD5:1001:0:192:168:254:12 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:12 soft-reconfiguration i                                                                       nbound
  neighbor FD00:3CD5:1001:0:192:168:254:13 activate
  neighbor FD00:3CD5:1001:0:192:168:254:13 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:13 soft-reconfiguration i                                                                       nbound
  neighbor FD00:3CD5:1001:0:192:168:254:14 activate
  neighbor FD00:3CD5:1001:0:192:168:254:14 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:14 soft-reconfiguration i                                                                       nbound
 exit-address-family
```

R12
```
R12#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 neighbor 192.168.254.15 remote-as 1001
 neighbor 192.168.254.15 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:15 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:15 update-source Loopback1
 !
 address-family ipv4
  neighbor 192.168.254.15 activate
  neighbor 192.168.254.15 soft-reconfiguration inbound
  no neighbor FD00:3CD5:1001:0:192:168:254:15 activate
 exit-address-family
 !
 address-family ipv6
  neighbor FD00:3CD5:1001:0:192:168:254:15 activate
  neighbor FD00:3CD5:1001:0:192:168:254:15 soft-reconfiguration i                                                                       nbound
 exit-address-family
```

R13
```
R13#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 neighbor 192.168.254.15 remote-as 1001
 neighbor 192.168.254.15 update-source Loopback1
 neighbor FD00:3CD5:1001:0:192:168:254:15 remote-as 1001
 neighbor FD00:3CD5:1001:0:192:168:254:15 update-source Loopback1
 !
 address-family ipv4
  neighbor 192.168.254.15 activate
  neighbor 192.168.254.15 soft-reconfiguration inbound
  no neighbor FD00:3CD5:1001:0:192:168:254:15 activate
 exit-address-family
 !
 address-family ipv6
  neighbor FD00:3CD5:1001:0:192:168:254:15 activate
  neighbor FD00:3CD5:1001:0:192:168:254:15 soft-reconfiguration i                                                                       nbound
 exit-address-family
```


Установление соседства R15:
```
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
100.1.0.6       4          301      19      11       20    0    0 00:06:10       17
192.168.254.12  4         1001      10      15       20    0    0 00:05:57        0
192.168.254.13  4         1001      10      15       20    0    0 00:05:52        0
192.168.254.14  4         1001      10      15       20    0    0 00:06:03        2

Neighbor                        V   AS      MsgRcvd MsgSent   TblVer    InQ OutQ    Up/Down  State/PfxRcd
2001:DB8:1001:100:1:0:4:6       4    301      54      47       20         0    0    00:38:33       17
FD00:3CD5:1001:0:192:168:254:12 4   1001      46      52       20         0    0    00:38:38        0
FD00:3CD5:1001:0:192:168:254:13 4   1001      45      52       20         0    0    00:38:36        0
FD00:3CD5:1001:0:192:168:254:14 4   1001      47      52       20         0    0    00:38:46        2
```

Cоседство установлено.


### Настроите iBGP в провайдере Триада

В качестве RR выбран маршрутизатор R24, остальные - его клиенты.

R24:
```
R24#sh run | sec bgp
router bgp 520
 bgp cluster-id 192.168.254.24
 bgp log-neighbor-changes
 neighbor 20.42.0.1 remote-as 2042
 neighbor 30.1.0.1 remote-as 301
 neighbor 2001:DB8:301:30:1::1 remote-as 301
 neighbor 2001:DB8:2042:20:42::1 remote-as 2042
 neighbor 192.168.254.23 remote-as 520
 neighbor 192.168.254.23 update-source Loopback1
 neighbor 192.168.254.25 remote-as 520
 neighbor 192.168.254.25 update-source Loopback1
 neighbor 192.168.254.26 remote-as 520
 neighbor 192.168.254.26 update-source Loopback1
 neighbor FD00:3CD5:520:0:192:168:254:23 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:23 update-source Loopback1
 neighbor FD00:3CD5:520:0:192:168:254:25 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:25 update-source Loopback1
 neighbor FD00:3CD5:520:0:192:168:254:26 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:26 update-source Loopback1
 !
 address-family ipv4
  network 20.42.0.0 mask 255.255.255.252
  network 30.1.0.0 mask 255.255.255.252
  network 192.168.254.24 mask 255.255.255.255
  neighbor 20.42.0.1 activate
  neighbor 30.1.0.1 activate
  no neighbor 2001:DB8:301:30:1::1 activate
  no neighbor 2001:DB8:2042:20:42::1 activate
  neighbor 192.168.254.23 activate
  neighbor 192.168.254.23 route-reflector-client
  neighbor 192.168.254.23 next-hop-self
  neighbor 192.168.254.23 soft-reconfiguration inbound
  neighbor 192.168.254.25 activate
  neighbor 192.168.254.25 route-reflector-client
  neighbor 192.168.254.25 next-hop-self
  neighbor 192.168.254.25 soft-reconfiguration inbound
  neighbor 192.168.254.26 activate
  neighbor 192.168.254.26 route-reflector-client
  neighbor 192.168.254.26 next-hop-self
  neighbor 192.168.254.26 soft-reconfiguration inbound
  no neighbor FD00:3CD5:520:0:192:168:254:23 activate
  no neighbor FD00:3CD5:520:0:192:168:254:25 activate
  no neighbor FD00:3CD5:520:0:192:168:254:26 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:301:30:1::/112
  network 2001:DB8:2042:20:42::/112
  network FD00:3CD5:520:0:192:168:254:24/128
  neighbor 2001:DB8:301:30:1::1 activate
  neighbor 2001:DB8:2042:20:42::1 activate
  neighbor FD00:3CD5:520:0:192:168:254:23 activate
  neighbor FD00:3CD5:520:0:192:168:254:23 route-reflector-client
  neighbor FD00:3CD5:520:0:192:168:254:23 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:23 soft-reconfiguration inbound
  neighbor FD00:3CD5:520:0:192:168:254:25 activate
  neighbor FD00:3CD5:520:0:192:168:254:25 route-reflector-client
  neighbor FD00:3CD5:520:0:192:168:254:25 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:25 soft-reconfiguration inbound
  neighbor FD00:3CD5:520:0:192:168:254:26 activate
  neighbor FD00:3CD5:520:0:192:168:254:26 route-reflector-client
  neighbor FD00:3CD5:520:0:192:168:254:26 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:26 soft-reconfiguration inbound
 exit-address-family
```


R23:
```
R23#sh run | sec bgp
router bgp 520
 bgp log-neighbor-changes
 neighbor 2001:DB8:101:101::1 remote-as 101
 neighbor 101.0.0.1 remote-as 101
 neighbor 192.168.254.24 remote-as 520
 neighbor 192.168.254.24 update-source Loopback1
 neighbor FD00:3CD5:520:0:192:168:254:24 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:24 update-source Loopback1
 !
 address-family ipv4
  network 101.0.0.0 mask 255.255.255.252
  network 192.168.254.23 mask 255.255.255.255
  no neighbor 2001:DB8:101:101::1 activate
  neighbor 101.0.0.1 activate
  neighbor 101.0.0.1 soft-reconfiguration inbound
  neighbor 192.168.254.24 activate
  neighbor 192.168.254.24 next-hop-self
  neighbor 192.168.254.24 soft-reconfiguration inbound
  no neighbor FD00:3CD5:520:0:192:168:254:24 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:101:101::/112
  network FD00:3CD5:520:0:192:168:254:23/128
  neighbor 2001:DB8:101:101::1 activate
  neighbor 2001:DB8:101:101::1 soft-reconfiguration inbound
  neighbor FD00:3CD5:520:0:192:168:254:24 activate
  neighbor FD00:3CD5:520:0:192:168:254:24 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:24 soft-reconfiguration inbound
 exit-address-family
```


R25:
```
R25#sh run | sec bgp
router bgp 520
 bgp log-neighbor-changes
 neighbor 192.168.254.24 remote-as 520
 neighbor 192.168.254.24 update-source Loopback1
 neighbor FD00:3CD5:520:0:192:168:254:24 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:24 update-source Loopback1
 !
 address-family ipv4
  network 52.0.0.0 mask 255.255.255.252
  network 52.0.0.4 mask 255.255.255.252
  network 192.168.254.25 mask 255.255.255.255
  neighbor 192.168.254.24 activate
  neighbor 192.168.254.24 next-hop-self
  neighbor 192.168.254.24 soft-reconfiguration inbound
  no neighbor FD00:3CD5:520:0:192:168:254:24 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:520:52::/112
  network 2001:DB8:520:52::4:0/112
  network FD00:3CD5:520:0:192:168:254:25/128
  neighbor FD00:3CD5:520:0:192:168:254:24 activate
  neighbor FD00:3CD5:520:0:192:168:254:24 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:24 soft-reconfiguration in                                                                       bound
 exit-address-family
```


R26:
```
R26#sh run | sec bgp
router bgp 520
 bgp log-neighbor-changes
 neighbor 20.42.0.5 remote-as 2042
 neighbor 2001:DB8:2042:20:42:0:4:5 remote-as 2042
 neighbor 192.168.254.24 remote-as 520
 neighbor FD00:3CD5:520:0:192:168:254:24 remote-as 520
 !
 address-family ipv4
  network 20.42.0.4 mask 255.255.255.252
  network 52.0.0.8 mask 255.255.255.252
  network 192.168.254.26 mask 255.255.255.255
  neighbor 20.42.0.5 activate
  neighbor 20.42.0.5 soft-reconfiguration inbound
  no neighbor 2001:DB8:2042:20:42:0:4:5 activate
  neighbor 192.168.254.24 activate
  neighbor 192.168.254.24 next-hop-self
  neighbor 192.168.254.24 soft-reconfiguration inbound
  no neighbor FD00:3CD5:520:0:192:168:254:24 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:520:52::8:0/112
  network 2001:DB8:2042:20:42:0:4:0/112
  network FD00:3CD5:520:0:192:168:254:26/128
  neighbor 2001:DB8:2042:20:42:0:4:5 activate
  neighbor 2001:DB8:2042:20:42:0:4:5 soft-reconfiguration inbound
  neighbor FD00:3CD5:520:0:192:168:254:24 activate
  neighbor FD00:3CD5:520:0:192:168:254:24 next-hop-self
  neighbor FD00:3CD5:520:0:192:168:254:24 soft-reconfiguration in                                                                       bound
 exit-address-family
```

Установление соседства R24:
```
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
20.42.0.1       4         2042    9769    9751      108    0    0 6d02h           3
30.1.0.1        4          301    9734    9738      108    0    0 6d02h           9
192.168.254.23  4          520    1446    1449      108    0    0 21:36:15        5
192.168.254.25  4          520    1399    1418      108    0    0 21:00:13        3
192.168.254.26  4          520    1442    1454      108    0    0 21:34:26        4

Neighbor                        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2001:DB8:301:30:1::1            4   301    9349    9342       56    0    0 5d21h           9
2001:DB8:2042:20:42::1          4   2042   9336    9362       56    0    0 5d21h           3
FD00:3CD5:520:0:192:168:254:23  4   520     926     928       56    0    0 13:48:37        5
FD00:3CD5:520:0:192:168:254:25  4   520     901     915       56    0    0 13:35:46        3
FD00:3CD5:520:0:192:168:254:26  4   520     916     929       56    0    0 13:45:19        4
```


Соседство установлено.

## Настройка Multihop в офисе С.-Петербург

Для того чтобы трафик до любого офиса распределялся по двум линкам одновременно, 
на пограничном маршуртизаторе офиса С.-Петербург R18 использовалась настройка multihop.


R18:
```
R18#sh run | sec bgp
router bgp 2042
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 neighbor 20.42.0.2 remote-as 520
 neighbor 20.42.0.6 remote-as 520
 neighbor 2001:DB8:2042:20:42::2 remote-as 520
 neighbor 2001:DB8:2042:20:42:0:4:6 remote-as 520
 !
 address-family ipv4
  network 20.42.0.0 mask 255.255.255.252
  network 20.42.0.4 mask 255.255.255.252
  network 192.168.254.18 mask 255.255.255.255
  neighbor 20.42.0.2 activate
  neighbor 20.42.0.2 soft-reconfiguration inbound
  neighbor 20.42.0.6 activate
  neighbor 20.42.0.6 soft-reconfiguration inbound
  no neighbor 2001:DB8:2042:20:42::2 activate
  no neighbor 2001:DB8:2042:20:42:0:4:6 activate
  maximum-paths 2
 exit-address-family
 !
  address-family ipv6
  maximum-paths 2
  network 2001:DB8:2042:20:42::/112
  network 2001:DB8:2042:20:42:0:4:0/112
  network FD00:3CD5:2042:0:192:168:254:18/128
  neighbor 2001:DB8:2042:20:42::2 activate
  neighbor 2001:DB8:2042:20:42::2 soft-reconfiguration inbound
  neighbor 2001:DB8:2042:20:42:0:4:6 activate
  neighbor 2001:DB8:2042:20:42:0:4:6 soft-reconfiguration inbound
 exit-address-family
```

## Проверка полченных результатов

Таблица BGP на R18 IPv4:
```
     Network                Next Hop            Metric LocPrf Weight Path
 *   20.42.0.0/30           20.42.0.6                              0 520 i
 *                          20.42.0.2                0             0 520 i
 *>                         0.0.0.0                  0         32768 i
 *>  20.42.0.4/30           0.0.0.0                  0         32768 i
 *                          20.42.0.6                0             0 520 i
 *                          20.42.0.2                              0 520 i
 *m  30.1.0.0/30            20.42.0.6                              0 520 i
 *>                         20.42.0.2                0             0 520 i
 *m  52.0.0.0/30            20.42.0.6                              0 520 i
 *>                         20.42.0.2                              0 520 i
 *m  52.0.0.4/30            20.42.0.2                              0 520 i
 *>                         20.42.0.6                              0 520 i
 *m  52.0.0.8/30            20.42.0.6                0             0 520 i
 *>                         20.42.0.2                              0 520 i
 *m  100.1.0.0/30           20.42.0.6                              0 520 101 i
 *>                         20.42.0.2                              0 520 101 i
 *m  100.1.0.4/30           20.42.0.6                              0 520 301 i
 *>                         20.42.0.2                              0 520 301 i
 *m  101.0.0.0/30           20.42.0.6                              0 520 i
 *>                         20.42.0.2                              0 520 i
 *m  101.0.0.4/30           20.42.0.6                              0 520 301 i
 *>                         20.42.0.2                              0 520 301 i
 *m  192.168.254.14/32      20.42.0.6                              0 520 301 1001 i
 *>                         20.42.0.2                              0 520 301 1001 i
 *m  192.168.254.15/32      20.42.0.6                              0 520 301 1001 i
 *>                         20.42.0.2                              0 520 301 1001 i
 *>  192.168.254.18/32      0.0.0.0                  0         32768 i
 *m  192.168.254.21/32      20.42.0.6                              0 520 301 i
 *>                         20.42.0.2                              0 520 301 i
 *m  192.168.254.22/32      20.42.0.6                              0 520 101 i
 *>                         20.42.0.2                              0 520 101 i
 *m  192.168.254.23/32      20.42.0.6                              0 520 i
 *>                         20.42.0.2                              0 520 i
 *m  192.168.254.24/32      20.42.0.6                              0 520 i
 *>                         20.42.0.2                0             0 520 i
 *m  192.168.254.25/32      20.42.0.2                              0 520 i
 *>                         20.42.0.6                              0 520 i
 *m  192.168.254.26/32      20.42.0.6                0             0 520 i
 *>                         20.42.0.2                              0 520 i
```


Таблица BGP на R18 IPv6:
```
     Network          Next Hop            Metric LocPrf Weight Path
 *m  2001:DB8:101:101::/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 i
 *m  2001:DB8:101:101::4:0/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 301 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *m  2001:DB8:301:30:1::/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                0             0 520 i
 *m  2001:DB8:520:52::/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 i
 *m  2001:DB8:520:52::4:0/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 i
 *m  2001:DB8:520:52::8:0/112
                       2001:DB8:2042:20:42::2
                                                              0 520 i
 *>                   2001:DB8:2042:20:42:0:4:6
                                                0             0 520 i
 *m  2001:DB8:1001:100:1::/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 101 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 101 i
 *m  2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 301 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *   2001:DB8:2042:20:42::/112
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   ::                       0         32768 i
 *                    2001:DB8:2042:20:42::2
                                                0             0 520 i
 *   2001:DB8:2042:20:42:0:4:0/112
                       2001:DB8:2042:20:42::2
                                                              0 520 i
 *                    2001:DB8:2042:20:42:0:4:6
                                                0             0 520 i
 *>                   ::                       0         32768 i
 *m  FD00:3CD5:101:0:192:168:254:22/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 101 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 101 i
 *m  FD00:3CD5:301:0:192:168:254:21/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 301 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *m  FD00:3CD5:520:0:192:168:254:23/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 i
 *m  FD00:3CD5:520:0:192:168:254:24/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                0             0 520 i
 *m  FD00:3CD5:520:0:192:168:254:25/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 i
 *m  FD00:3CD5:520:0:192:168:254:26/128
                       2001:DB8:2042:20:42::2
                                                              0 520 i
 *>                   2001:DB8:2042:20:42:0:4:6
                                                0             0 520 i
 *m  FD00:3CD5:1001:0:192:168:254:14/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 301 1001 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 301 1001 i
 *m  FD00:3CD5:1001:0:192:168:254:15/128
                       2001:DB8:2042:20:42:0:4:6
                                                              0 520 301 1001 i
 *>                   2001:DB8:2042:20:42::2
                                                              0 520 301 1001 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       ::                       0         32768 i
```


Таблица BGP на R14 IPv4:
```
     Network            Next Hop            Metric LocPrf Weight Path
 *>i 20.42.0.0/30       100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 20.42.0.4/30       100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 30.1.0.0/30        100.1.0.6                0    150      0 301 i
 *                      100.1.0.2                              0 101 301 i
 *>i 52.0.0.0/30        100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 52.0.0.4/30        100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 52.0.0.8/30        100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 * i 100.1.0.0/30       100.1.0.6                0    150      0 301 101 i
 *                      100.1.0.2                0             0 101 i
 *>                     0.0.0.0                  0         32768 i
 r>i 100.1.0.4/30       192.168.254.15           0    100      0 i
 r                      100.1.0.2                              0 101 301 i
 *>i 101.0.0.0/30       100.1.0.6                0    150      0 301 101 i
 *                      100.1.0.2                0             0 101 i
 *>i 101.0.0.4/30       100.1.0.6                0    150      0 301 i
 *                      100.1.0.2                0             0 101 i
 *>  192.168.254.14/32  0.0.0.0                  0         32768 i
 r>i 192.168.254.15/32  192.168.254.15           0    100      0 i
 *>i 192.168.254.18/32  100.1.0.6                0    150      0 301 520 2042 i
 *                      100.1.0.2                              0 101 520 2042 i
 *>i 192.168.254.21/32  100.1.0.6                0    150      0 301 i
 *                      100.1.0.2                              0 101 301 i
 *>i 192.168.254.22/32  100.1.0.6                0    150      0 301 101 i
 *                      100.1.0.2                0             0 101 i
 *>i 192.168.254.23/32  100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 192.168.254.24/32  100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 192.168.254.25/32  100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
 *>i 192.168.254.26/32  100.1.0.6                0    150      0 301 520 i
 *                      100.1.0.2                              0 101 520 i
```

Таблица BGP на R14 IPv6:
```
     Network          Next Hop            Metric LocPrf Weight Path
 *>i 2001:DB8:101:101::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 101 i
 *                    2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>i 2001:DB8:101:101::4:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 i
 *                    2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>i 2001:DB8:301:30:1::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *>i 2001:DB8:520:52::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i 2001:DB8:520:52::4:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i 2001:DB8:520:52::8:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 * i 2001:DB8:1001:100:1::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 101 i
 *                    2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>                   ::                       0         32768 i
 r>i 2001:DB8:1001:100:1:0:4:0/112
                       FD00:3CD5:1001:0:192:168:254:15
                                                0    100      0 i
 r                    2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *>i 2001:DB8:2042:20:42::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i 2001:DB8:2042:20:42:0:4:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i FD00:3CD5:101:0:192:168:254:22/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 101 i
 *                    2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>i FD00:3CD5:301:0:192:168:254:21/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *>i FD00:3CD5:520:0:192:168:254:23/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i FD00:3CD5:520:0:192:168:254:24/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i FD00:3CD5:520:0:192:168:254:25/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>i FD00:3CD5:520:0:192:168:254:26/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 i
 *>  FD00:3CD5:1001:0:192:168:254:14/128
                       ::                       0         32768 i
 r>i FD00:3CD5:1001:0:192:168:254:15/128
                       FD00:3CD5:1001:0:192:168:254:15
                                                0    100      0 i
 *>i FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:1001:100:1:0:4:6
                                                0    150      0 301 520 2042 i
 *                    2001:DB8:1001:100:1::2
                                                              0 101 520 2042 i
```

Трассировка IPv4 с R14 на R18, R27, R28:

```
R14#traceroute 192.168.254.18 source lo1
Type escape sequence to abort.
Tracing the route to 192.168.254.18
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 1 msec 0 msec 0 msec
  2 100.1.0.6 2 msec 0 msec 0 msec
  3 30.1.0.2 [AS 301] 1 msec 1 msec 2 msec
  4 20.42.0.1 [AS 520] 2 msec *  2 msec

R14#traceroute 52.0.0.2 source lo1
Type escape sequence to abort.
Tracing the route to 52.0.0.2
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 2 msec 0 msec 1 msec
  2 100.1.0.6 1 msec 1 msec 1 msec
  3 30.1.0.2 [AS 301] 1 msec 1 msec 2 msec
  4 172.16.2.10 1 msec 2 msec 1 msec
  5 172.16.2.14 2 msec 2 msec 2 msec
  6 52.0.0.2 [AS 520] 2 msec *  3 msec

R14#traceroute 52.0.0.6 source lo1
Type escape sequence to abort.
Tracing the route to 52.0.0.6
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 1 msec 1 msec 0 msec
  2 100.1.0.6 1 msec 1 msec 1 msec
  3 30.1.0.2 [AS 301] 2 msec 1 msec 2 msec
  4 172.16.2.5 10 msec 2 msec 1 msec
  5 172.16.2.2 3 msec 4 msec 2 msec
  6 52.0.0.6 [AS 520] 3 msec *  2 msec
```


Трассировка IPv6 с R14 на R18, R27, R28:

```
R14#traceroute ipv6
Target IPv6 address: FD00:3CD5:2042:0:192:168:254:18
Source address: FD00:3CD5:1001:0:192:168:254:14
Insert source routing header? [no]:
Numeric display? [no]:
Timeout in seconds [3]:
Probe count [3]:
Minimum Time to Live [1]:
Maximum Time to Live [30]:
Priority [0]:
Port Number [0]:
Type escape sequence to abort.
Tracing the route to FD00:3CD5:2042:0:192:168:254:18

  1 FD00:3CD5:1001:172:16:1:44:46 1 msec 1 msec 0 msec
  2 2001:DB8:1001:100:1:0:4:6 1 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 1 msec 2 msec 2 msec
  4 2001:DB8:2042:20:42::1 [AS 520] 20 msec 20 msec 1 msec

R14#traceroute ipv6
Target IPv6 address: 2001:DB8:520:52::2
Source address: FD00:3CD5:1001:0:192:168:254:14
Insert source routing header? [no]:
Numeric display? [no]:
Timeout in seconds [3]:
Probe count [3]:
Minimum Time to Live [1]:
Maximum Time to Live [30]:
Priority [0]:
Port Number [0]:
Type escape sequence to abort.
Tracing the route to 2001:DB8:520:52::2

  1 FD00:3CD5:1001:172:16:1:44:46 1 msec 0 msec 0 msec
  2 2001:DB8:1001:100:1:0:4:6 1 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 2 msec 1 msec 1 msec
  4 FD00:3CD5:520:172:16:2:8:10 1 msec 3 msec 1 msec
  5 FD00:3CD5:520:172:16:2:12:14 2 msec 3 msec 2 msec
  6 2001:DB8:520:52::2 [AS 520] 2 msec 3 msec 3 msec

R14#traceroute ipv6
Target IPv6 address:  2001:DB8:520:52::4:6
Source address: FD00:3CD5:1001:0:192:168:254:14
Insert source routing header? [no]:
Numeric display? [no]:
Timeout in seconds [3]:
Probe count [3]:
Minimum Time to Live [1]:
Maximum Time to Live [30]:
Priority [0]:
Port Number [0]:
Type escape sequence to abort.
Tracing the route to 2001:DB8:520:52::4:6

  1 FD00:3CD5:1001:172:16:1:44:46 1 msec 0 msec 0 msec
  2 2001:DB8:1001:100:1:0:4:6 2 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 2 msec 1 msec 3 msec
  4 FD00:3CD5:520:172:16:2:8:10 2 msec 2 msec 2 msec
  5 FD00:3CD5:520:172:16:2:12:14 2 msec 2 msec 2 msec
  6 2001:DB8:520:52::4:6 [AS 520] 2 msec 2 msec 2 msec
```

IP-связность сети достигнута.