# BGP. Основы

----

## Задание:

* Настроить eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
* Настроить eBGP между провайдерами Киторн и Ламас.
* Настроить eBGP между Ламас и Триада.
* Настроить eBGP между офисом С.-Петербург и провайдером Триада.
* Организовать IP доступность между пограничными роутерами офисов Москва и С.-Петербург.


### Схема реализации (EVE-NG)
\
![BigNet.png](img%2FBigNet.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).

### План адресации IPv4/IPv6



|           |           |                  |                |     AS1001      |                                |               |                      |               |
|:----------------|:----------|:-----------------|:--------------:|:---------------:|:------------------------------:|:-------------:|:--------------------:|:-------------:|
|           |           |                  |                |     Москва      |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |         IPv6 GUA / ULA         | Prefix length |       IPv6 LLA       | Prefix length |
| R14             | e0/0      | To_R12           |  172.16.1.13   |       /30       | FD00:3CD5:1001:172:16:1:12:13  |     /112      |  fe80::172:16:1:13   | /10           |
|                 | e0/1      | To_R13           |  172.16.1.17   |       /30       | FD00:3CD5:1001:172:16:1:16:17  |     /112      |  fe80::172:16:1:17   | /10           |
|                 | e0/2      | To_R22_AS101     |   100.1.0.1    |       /30       |     2001:DB8:1001:100:1::1     |     /112      |   fe80::100:1:0:1    | /10           |
|                 | e0/3      | To_R19           |  172.16.1.21   |       /30       | FD00:3CD5:1001:172:16:1:20:21  |     /112      |  fe80::172:16:1:21   | /10           |
|                 | Lo1       | Lo1              | 192.168.254.14 |       /32       | FD00:3CD5:1001::192:168:254:14 |     /128      | fe80::192:168:254:14 | /10           |
| R15             | e0/0      | To_R13           |   172.16.1.1   |       /30       |   FD00:3CD5:1001:172:16:1::1   |     /112      |   fe80::172:16:1:1   | /10           |
|                 | e0/1      | To_R12           |   172.16.1.5   |       /30       |  FD00:3CD5:1001:172:16:1:4:5   |     /112      |   fe80::172:16:1:5   | /10           |
|                 | e0/2      | To_R21_AS301     |   100.1.0.5    |       /30       |    2001:DB8:1001:100:1::4:5    |     /112      |   fe80::100:1:0:5    | /10           |
|                 | e0/3      | To_R20           |   172.16.1.9   |       /30       |  FD00:3CD5:1001:172:16:1:8:9   |     /112      |   fe80::172:16:1:9   | /10           |
|                 | Lo1       | Lo1              | 192.168.254.15 |       /32       | FD00:3CD5:1001::192:168:254:15 |     /128      | fe80::192:168:254:15 | /10           |
|            |           |                  |                |      AS101      |                                |               |                      |               |
|           |           |                  |                |     Киторн      |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R22             | e0/0      | To_R14_AS1001    |   100.1.0.2    |       /30       |     2001:DB8:1001:100:1::2     |     /112      |   fe80::100:1:0:2    | /10           |
|                 | e0/1      | To_R21_AS301     |   101.0.0.5    |       /30       |     2001:DB8:101:101::4:5      |     /112      |   fe80::101:0:0:5    | /10           |
|                 | e0/2      | To_R23_AS520     |   101.0.0.1    |       /30       |      2001:DB8:101:101::1       |     /112      |   fe80::101:0:0:1    | /10           |
|                 | Lo1       | Lo1              | 192.168.254.22 |       /32       | FD00:3CD5:101::192:168:254:22  |     /128      | fe80::192:168:254:22 | /10           |
|            |           |                  |                |      AS301      |                                |               |                      |               |
|            |           |                  |                |      Ламас      |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R21             | e0/0      | To_R15_AS1001    |   100.1.0.6    |       /30       |    2001:DB8:1001:100:1::4:6    |     /112      |   fe80::100:1:0:6    | /10           |
|                 | e0/1      | To_R22_AS101     |   101.0.0.6    |       /30       |     2001:DB8:101:101::4:6      |     /112      |   fe80::101:0:0:6    | /10           |
|                 | e0/2      | To_R24_AS520     |    30.1.0.1    |       /30       |      2001:DB8:301:30:1::1      |     /112      |    fe80::30:1:0:1    | /10           |
|                 | Lo1       | Lo1              | 192.168.254.21 |       /32       | FD00:3CD5:301::192:168:254:21  |     /128      | fe80::192:168:254:21 | /10           |
|            |           |                  |                |      AS520      |                                |               |                      |               |
|           |           |                  |                |     Триада      |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R24             | e0/0      | To_R21_AS301     |    30.1.0.2    |       /30       |      2001:DB8:301:30:1::2      |     /112      |    fe80::30:1:0:2    | /10           |
|                 | e0/1      | To_R26           |   172.16.2.9   |       /30       |   FD00:3CD5:520:172:16:2:8:9   |     /112      |   fe80::172:16:2:9   | /10           |
|                 | e0/2      | To_R23           |   172.16.2.6   |       /30       |   FD00:3CD5:520:172:16:2:4:6   |     /112      |   fe80::172:16:2:6   | /10           |
|                 | e0/3      | To_R18_AS2042    |   20.42.0.2    |       /30       |     2001:DB8:2042:20:42::2     |     /112      |   fe80::20:42:0:2    | /10           |
|                 | Lo1       | Lo1              | 192.168.254.24 |       /32       | FD00:3CD5:520::192:168:254:24  |     /128      | fe80::192:168:254:24 | /10           |
|           |           |                  |                |     AS2042      |                                |               |                      |               |
|  |           |                  |                | Санкт-Петербург |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| R18             | e0/0      | To_R16           |   172.16.3.1   |       /30       |   FD00:3CD5:2042:172:16:3::1   |     /112      |   fe80::172:16:3:1   | /10           |
|                 | e0/1      | To_R17           |   172.16.3.5   |       /30       |  FD00:3CD5:2042:172:16:3:4:5   |     /112      |   fe80::172:16:3:5   | /10           |
|                 | e0/2      | To_R24_AS520     |   20.42.0.1    |       /30       |     2001:DB8:2042:20:42::1     |     /112      |   fe80::20:42:0:1    | /10           |
|                 | e0/3      | To_R26_AS520     |   20.42.0.5    |       /30       |    2001:DB8:2042:20:42::4:5    |     /112      |   fe80::20:42:0:5    | /10           |
|                 | Lo1       | Lo1              | 192.168.254.18 |       /32       | FD00:3CD5:2042::192:168:254:18 |     /128      | fe80::192:168:254:18 | /10           |



### Настройка eBGP между офисами

Пограничные маршрутизаторы офиса Москва R14 и R15 имеют P2P линки в сторону офисов Киторн (AS101, R22)
и Ламас (AS301, R21) соответственно. Офис Ламас и офис Триада соединены линка R21 - R24.
Офисы С.-Петербурга и Триады связаны линком R24 - R18.
Настроим eBGP сессии между R14 - R22, R15 - R21, R21 - R22, R21 - R24, R24 - R18 и проанонсим
P2P-сети и адреса Loopback-интерфейсов для проверки доступности.

R14:
```
R14#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 neighbor 2001:DB8:1001:100:1::2 remote-as 101
 neighbor 100.1.0.2 remote-as 101
 !
 address-family ipv4
  network 100.1.0.0 mask 255.255.255.252
  network 192.168.254.14 mask 255.255.255.255
  no neighbor 2001:DB8:1001:100:1::2 activate
  neighbor 100.1.0.2 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:1001:100:1::/112
  network FD00:3CD5:1001:0:192:168:254:14/128
  neighbor 2001:DB8:1001:100:1::2 activate
 exit-address-family
```

R15:
```
router bgp 1001
 bgp log-neighbor-changes
 neighbor 2001:DB8:1001:100:1:0:4:6 remote-as 301
 neighbor 100.1.0.6 remote-as 301
 !
 address-family ipv4
  network 100.1.0.4 mask 255.255.255.252
  network 192.168.254.15 mask 255.255.255.255
  no neighbor 2001:DB8:1001:100:1:0:4:6 activate
  neighbor 100.1.0.6 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:1001:100:1:0:4:0/112
  network FD00:3CD5:1001:0:192:168:254:15/128
  neighbor 2001:DB8:1001:100:1:0:4:6 activate
 exit-address-family
```

R21
```
router bgp 301
 bgp log-neighbor-changes
 neighbor 30.1.0.2 remote-as 520
 neighbor 2001:DB8:101:101::4:5 remote-as 101
 neighbor 2001:DB8:301:30:1::2 remote-as 520
 neighbor 2001:DB8:1001:100:1:0:4:5 remote-as 1001
 neighbor 100.1.0.5 remote-as 1001
 neighbor 101.0.0.5 remote-as 101
 !
 address-family ipv4
  network 30.1.0.0 mask 255.255.255.252
  network 100.1.0.4 mask 255.255.255.252
  network 101.0.0.4 mask 255.255.255.252
  network 192.168.254.21 mask 255.255.255.255
  neighbor 30.1.0.2 activate
  no neighbor 2001:DB8:101:101::4:5 activate
  no neighbor 2001:DB8:301:30:1::2 activate
  no neighbor 2001:DB8:1001:100:1:0:4:5 activate
  neighbor 100.1.0.5 activate
  neighbor 101.0.0.5 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:101:101::4:0/112
  network 2001:DB8:301:30:1::/112
  network 2001:DB8:1001:100:1:0:4:0/112
  network FD00:3CD5:301:0:192:168:254:21/128
  neighbor 2001:DB8:101:101::4:5 activate
  neighbor 2001:DB8:301:30:1::2 activate
  neighbor 2001:DB8:1001:100:1:0:4:5 activate
 exit-address-family
```

R22
```
router bgp 101
 bgp log-neighbor-changes
 neighbor 2001:DB8:101:101::4:6 remote-as 301
 neighbor 2001:DB8:1001:100:1::1 remote-as 1001
 neighbor 100.1.0.1 remote-as 1001
 neighbor 101.0.0.6 remote-as 301
 !
 address-family ipv4
  network 100.1.0.0 mask 255.255.255.252
  network 101.0.0.4 mask 255.255.255.252
  network 192.168.254.22 mask 255.255.255.255
  no neighbor 2001:DB8:101:101::4:6 activate
  no neighbor 2001:DB8:1001:100:1::1 activate
  neighbor 100.1.0.1 activate
  neighbor 101.0.0.6 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:101:101::4:0/112
  network 2001:DB8:1001:100:1::/112
  network FD00:3CD5:101:0:192:168:254:22/128
  neighbor 2001:DB8:101:101::4:6 activate
  neighbor 2001:DB8:1001:100:1::1 activate
 exit-address-family
```

R24:
```
router bgp 520
 bgp log-neighbor-changes
 neighbor 20.42.0.1 remote-as 2042
 neighbor 30.1.0.1 remote-as 301
 neighbor 2001:DB8:301:30:1::1 remote-as 301
 neighbor 2001:DB8:2042:20:42::1 remote-as 2042
 !
 address-family ipv4
  network 20.42.0.0 mask 255.255.255.252
  network 30.1.0.0 mask 255.255.255.252
  network 192.168.254.24 mask 255.255.255.255
  neighbor 20.42.0.1 activate
  neighbor 30.1.0.1 activate
  no neighbor 2001:DB8:301:30:1::1 activate
  no neighbor 2001:DB8:2042:20:42::1 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:301:30:1::/112
  network 2001:DB8:2042:20:42::/112
  network FD00:3CD5:1001:0:192:168:254:24/128
  neighbor 2001:DB8:301:30:1::1 activate
  neighbor 2001:DB8:2042:20:42::1 activate
 exit-address-family
```

R18:
```
router bgp 2042
 bgp log-neighbor-changes
 neighbor 20.42.0.2 remote-as 520
 neighbor 2001:DB8:2042:20:42::2 remote-as 520
 !
 address-family ipv4
  network 20.42.0.0 mask 255.255.255.252
  network 192.168.254.18 mask 255.255.255.255
  neighbor 20.42.0.2 activate
  no neighbor 2001:DB8:2042:20:42::2 activate
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:2042:20:42::/112
  network FD00:3CD5:2042:0:192:168:254:18/128
  neighbor 2001:DB8:2042:20:42::2 activate
 exit-address-family
```


Установление соседства:
R14
```
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
100.1.0.2       4          101     421     399       61    0    0 05:48:39        9
```

R21
```
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
30.1.0.2        4          520     420     420       56    0    0 05:49:39        4
100.1.0.5       4         1001     397     419       56    0    0 05:49:37        2
101.0.0.5       4          101     419     421       56    0    0 05:49:37        4
```

R18
```
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
20.42.0.2       4          520     430     446       62    0    0 05:56:00       10
```

Как видим соседство установлено, префиксы анонсятся.


### Проверка доступности порганичных роутеров между офисами Москва и С.-Петербург

Проверим таблицы bgp и маршрутизации на пограничных роутерах в офисах Москва и С.-Петербург

R14:
```
R14#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>  20.42.0.0/30     100.1.0.2                              0 101 301 520 i
 *>  30.1.0.0/30      100.1.0.2                              0 101 301 i
 *   100.1.0.0/30     100.1.0.2                0             0 101 i
 *>                   0.0.0.0                  0         32768 i
 *>  100.1.0.4/30     100.1.0.2                              0 101 301 i
 *>  101.0.0.4/30     100.1.0.2                0             0 101 i
 *>  192.168.254.14/32
                       0.0.0.0                  0         32768 i
 *>  192.168.254.18/32
                       100.1.0.2                              0 101 301 520 2042 i
 *>  192.168.254.21/32
                       100.1.0.2                              0 101 301 i
 *>  192.168.254.22/32
                       100.1.0.2                0             0 101 i
 *>  192.168.254.24/32
                       100.1.0.2                              0 101 301 520 i

R14#sh ip bgp ipv6 unicast

     Network          Next Hop            Metric LocPrf Weight Path
 *>  2001:DB8:101:101::4:0/112
                       2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>  2001:DB8:301:30:1::/112
                       2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *   2001:DB8:1001:100:1::/112
                       2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>                   ::                       0         32768 i
 *>  2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *>  2001:DB8:2042:20:42::/112
                       2001:DB8:1001:100:1::2
                                                              0 101 301 520 i
 *>  FD00:3CD5:101:0:192:168:254:22/128
                       2001:DB8:1001:100:1::2
                                                0             0 101 i
 *>  FD00:3CD5:301:0:192:168:254:21/128
                       2001:DB8:1001:100:1::2
                                                              0 101 301 i
 *>  FD00:3CD5:1001:0:192:168:254:14/128
                       ::                       0         32768 i
 *>  FD00:3CD5:1001:0:192:168:254:24/128
                       2001:DB8:1001:100:1::2
                                                              0 101 301 520 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:1001:100:1::2
                                                              0 101 301 520 2042 i

R14#sh ip route

Gateway of last resort is 100.1.0.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 100.1.0.2
      20.0.0.0/30 is subnetted, 1 subnets
B        20.42.0.0 [20/0] via 100.1.0.2, 06:02:13
      30.0.0.0/30 is subnetted, 1 subnets
B        30.1.0.0 [20/0] via 100.1.0.2, 06:02:13
      100.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C        100.1.0.0/30 is directly connected, Ethernet0/2
L        100.1.0.1/32 is directly connected, Ethernet0/2
B        100.1.0.4/30 [20/0] via 100.1.0.2, 06:02:13
      101.0.0.0/30 is subnetted, 1 subnets
B        101.0.0.4 [20/0] via 100.1.0.2, 06:02:44
      172.16.0.0/16 is variably subnetted, 12 subnets, 3 masks
S        172.16.1.0/24 is directly connected, Null0
O        172.16.1.0/30 [110/30] via 172.16.1.14, 06:03:29, Ethernet0/0
O        172.16.1.4/30 [110/20] via 172.16.1.14, 06:03:29, Ethernet0/0
O IA     172.16.1.8/30 [110/30] via 172.16.1.14, 06:03:29, Ethernet0/0
C        172.16.1.12/30 is directly connected, Ethernet0/0
L        172.16.1.13/32 is directly connected, Ethernet0/0
C        172.16.1.16/30 is directly connected, Ethernet0/1
L        172.16.1.17/32 is directly connected, Ethernet0/1
C        172.16.1.20/30 is directly connected, Ethernet0/3
L        172.16.1.21/32 is directly connected, Ethernet0/3
O IA     172.16.1.32/30 [110/20] via 172.16.1.14, 06:03:29, Ethernet0/0
O IA     172.16.1.36/30 [110/20] via 172.16.1.14, 06:03:29, Ethernet0/0
      192.168.254.0/32 is subnetted, 9 subnets
O        192.168.254.12 [110/11] via 172.16.1.14, 06:03:29, Ethernet0/0
C        192.168.254.14 is directly connected, Loopback1
O        192.168.254.15 [110/21] via 172.16.1.14, 06:03:29, Ethernet0/0
B        192.168.254.18 [20/0] via 100.1.0.2, 06:01:42
O        192.168.254.19 [110/11] via 172.16.1.22, 06:03:29, Ethernet0/3
O IA     192.168.254.20 [110/31] via 172.16.1.14, 06:03:29, Ethernet0/0
B        192.168.254.21 [20/0] via 100.1.0.2, 06:02:13
B        192.168.254.22 [20/0] via 100.1.0.2, 06:02:44
B        192.168.254.24 [20/0] via 100.1.0.2, 06:02:13
```

R15:
```
R15#sh ip bgp ipv6 unicast

     Network          Next Hop            Metric LocPrf Weight Path
 *>  2001:DB8:101:101::4:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0             0 301 i
 *>  2001:DB8:301:30:1::/112
                       2001:DB8:1001:100:1:0:4:6
                                                0             0 301 i
 *>  2001:DB8:1001:100:1::/112
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 101 i
 *   2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:1001:100:1:0:4:6
                                                0             0 301 i
 *>                   ::                       0         32768 i
 *>  2001:DB8:2042:20:42::/112
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 520 i
 *>  FD00:3CD5:101:0:192:168:254:22/128
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 101 i
 *>  FD00:3CD5:301:0:192:168:254:21/128
                       2001:DB8:1001:100:1:0:4:6
                                                0             0 301 i
 *>  FD00:3CD5:1001:0:192:168:254:15/128
                       ::                       0         32768 i
 *>  FD00:3CD5:1001:0:192:168:254:24/128
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 520 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 520 2042 i

R15#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>  20.42.0.0/30     100.1.0.6                              0 301 520 i
 *>  30.1.0.0/30      100.1.0.6                0             0 301 i
 *>  100.1.0.0/30     100.1.0.6                              0 301 101 i
 *   100.1.0.4/30     100.1.0.6                0             0 301 i
 *>                   0.0.0.0                  0         32768 i
 *>  101.0.0.4/30     100.1.0.6                0             0 301 i
 *>  192.168.254.15/32
                       0.0.0.0                  0         32768 i
 *>  192.168.254.18/32
                       100.1.0.6                              0 301 520 2042 i
 *>  192.168.254.21/32
                       100.1.0.6                0             0 301 i
 *>  192.168.254.22/32
                       100.1.0.6                              0 301 101 i
 *>  192.168.254.24/32
                       100.1.0.6                              0 301 520 i

R15#sh ip route

Gateway of last resort is 100.1.0.6 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 100.1.0.6
      20.0.0.0/30 is subnetted, 1 subnets
B        20.42.0.0 [20/0] via 100.1.0.6, 06:03:58
      30.0.0.0/30 is subnetted, 1 subnets
B        30.1.0.0 [20/0] via 100.1.0.6, 06:03:58
      100.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        100.1.0.0/30 [20/0] via 100.1.0.6, 06:03:58
C        100.1.0.4/30 is directly connected, Ethernet0/2
L        100.1.0.5/32 is directly connected, Ethernet0/2
      101.0.0.0/30 is subnetted, 1 subnets
B        101.0.0.4 [20/0] via 100.1.0.6, 06:03:58
      172.16.0.0/16 is variably subnetted, 12 subnets, 3 masks
S        172.16.1.0/24 is directly connected, Null0
C        172.16.1.0/30 is directly connected, Ethernet0/0
L        172.16.1.1/32 is directly connected, Ethernet0/0
C        172.16.1.4/30 is directly connected, Ethernet0/1
L        172.16.1.5/32 is directly connected, Ethernet0/1
C        172.16.1.8/30 is directly connected, Ethernet0/3
L        172.16.1.9/32 is directly connected, Ethernet0/3
O        172.16.1.12/30 [110/20] via 172.16.1.6, 06:04:45, Ethernet0/1
O        172.16.1.16/30 [110/30] via 172.16.1.6, 06:04:35, Ethernet0/1
O IA     172.16.1.20/30 [110/30] via 172.16.1.6, 06:04:35, Ethernet0/1
O IA     172.16.1.32/30 [110/20] via 172.16.1.6, 06:04:45, Ethernet0/1
O IA     172.16.1.36/30 [110/20] via 172.16.1.6, 06:04:45, Ethernet0/1
      192.168.254.0/32 is subnetted, 9 subnets
O        192.168.254.12 [110/11] via 172.16.1.6, 06:04:45, Ethernet0/1
O        192.168.254.14 [110/21] via 172.16.1.6, 06:04:35, Ethernet0/1
C        192.168.254.15 is directly connected, Loopback1
B        192.168.254.18 [20/0] via 100.1.0.6, 06:03:27
O IA     192.168.254.19 [110/31] via 172.16.1.6, 06:04:35, Ethernet0/1
O        192.168.254.20 [110/11] via 172.16.1.10, 06:04:45, Ethernet0/3
B        192.168.254.21 [20/0] via 100.1.0.6, 06:03:58
B        192.168.254.22 [20/0] via 100.1.0.6, 06:03:58
B        192.168.254.24 [20/0] via 100.1.0.6, 06:03:58
```

R18:
```
R18#sh ip bgp ipv6 unicast

     Network          Next Hop            Metric LocPrf Weight Path
 *>  2001:DB8:101:101::4:0/112
                       2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *>  2001:DB8:301:30:1::/112
                       2001:DB8:2042:20:42::2
                                                0             0 520 i
 *>  2001:DB8:1001:100:1::/112
                       2001:DB8:2042:20:42::2
                                                              0 520 301 101 i
 *>  2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *>  2001:DB8:2042:20:42::/112
                       ::                       0         32768 i
 *                    2001:DB8:2042:20:42::2
                                                0             0 520 i
 *>  FD00:3CD5:101:0:192:168:254:22/128
                       2001:DB8:2042:20:42::2
                                                              0 520 301 101 i
 *>  FD00:3CD5:301:0:192:168:254:21/128
                       2001:DB8:2042:20:42::2
                                                              0 520 301 i
 *>  FD00:3CD5:1001:0:192:168:254:14/128
                       2001:DB8:2042:20:42::2
                                                              0 520 301 101 1001 i
 *>  FD00:3CD5:1001:0:192:168:254:15/128
                       2001:DB8:2042:20:42::2
                                                              0 520 301 1001 i
 *>  FD00:3CD5:1001:0:192:168:254:24/128
                       2001:DB8:2042:20:42::2
                                                0             0 520 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       ::                       0         32768 i

R18#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *   20.42.0.0/30     20.42.0.2                0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *>  30.1.0.0/30      20.42.0.2                0             0 520 i
 *>  100.1.0.0/30     20.42.0.2                              0 520 301 101 i
 *>  100.1.0.4/30     20.42.0.2                              0 520 301 i
 *>  101.0.0.4/30     20.42.0.2                              0 520 301 i
 *>  192.168.254.14/32
                       20.42.0.2                              0 520 301 101 1001 i
 *>  192.168.254.15/32
                       20.42.0.2                              0 520 301 1001 i
 *>  192.168.254.18/32
                       0.0.0.0                  0         32768 i
 *>  192.168.254.21/32
                       20.42.0.2                              0 520 301 i
 *>  192.168.254.22/32
                       20.42.0.2                              0 520 301 101 i
 *>  192.168.254.24/32
                       20.42.0.2                0             0 520 i
R18#sh ip route

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

S*    0.0.0.0/0 is directly connected, Null0
      10.0.0.0/24 is subnetted, 2 subnets
D        10.0.9.0 [90/1541120] via 172.16.3.6, 05:17:37, Ethernet0/1
                  [90/1541120] via 172.16.3.2, 05:17:37, Ethernet0/0
D        10.0.10.0 [90/1541120] via 172.16.3.6, 05:12:18, Ethernet0/1
                   [90/1541120] via 172.16.3.2, 05:12:18, Ethernet0/0
      20.0.0.0/8 is variably subnetted, 4 subnets, 2 masks
C        20.42.0.0/30 is directly connected, Ethernet0/2
L        20.42.0.1/32 is directly connected, Ethernet0/2
C        20.42.0.4/30 is directly connected, Ethernet0/3
L        20.42.0.5/32 is directly connected, Ethernet0/3
      30.0.0.0/30 is subnetted, 1 subnets
B        30.1.0.0 [20/0] via 20.42.0.2, 06:04:57
      100.0.0.0/30 is subnetted, 2 subnets
B        100.1.0.0 [20/0] via 20.42.0.2, 06:04:45
B        100.1.0.4 [20/0] via 20.42.0.2, 06:04:45
      101.0.0.0/30 is subnetted, 1 subnets
B        101.0.0.4 [20/0] via 20.42.0.2, 06:04:45
      172.16.0.0/16 is variably subnetted, 5 subnets, 3 masks
D        172.16.3.0/24 [90/1536000] via 172.16.3.6, 05:19:09, Ethernet0/1
                       [90/1536000] via 172.16.3.2, 05:19:09, Ethernet0/0
C        172.16.3.0/30 is directly connected, Ethernet0/0
L        172.16.3.1/32 is directly connected, Ethernet0/0
C        172.16.3.4/30 is directly connected, Ethernet0/1
L        172.16.3.5/32 is directly connected, Ethernet0/1
      192.168.254.0/32 is subnetted, 10 subnets
D        192.168.254.9 [90/1536640] via 172.16.3.6, 05:19:09, Ethernet0/1
                       [90/1536640] via 172.16.3.2, 05:19:09, Ethernet0/0
D        192.168.254.10 [90/1536640] via 172.16.3.6, 05:19:09, Ethernet0/1
                        [90/1536640] via 172.16.3.2, 05:19:09, Ethernet0/0
B        192.168.254.14 [20/0] via 20.42.0.2, 01:52:10
B        192.168.254.15 [20/0] via 20.42.0.2, 06:04:45
D        192.168.254.16 [90/1024640] via 172.16.3.2, 05:19:09, Ethernet0/0
D        192.168.254.17 [90/1024640] via 172.16.3.6, 05:19:09, Ethernet0/1
C        192.168.254.18 is directly connected, Loopback1
B        192.168.254.21 [20/0] via 20.42.0.2, 06:04:45
B        192.168.254.22 [20/0] via 20.42.0.2, 06:04:45
B        192.168.254.24 [20/0] via 20.42.0.2, 06:04:57
```

Проверка доступности:
```
R14#ping 192.168.254.18
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.254.18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R14#ping FD00:3CD5:2042:0:192:168:254:18
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to FD00:3CD5:2042:0:192:168:254:18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/3 ms

R15#ping 192.168.254.18
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.254.18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R15#ping FD00:3CD5:2042:0:192:168:254:18
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to FD00:3CD5:2042:0:192:168:254:18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

R18#ping 192.168.254.14
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.254.14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R18#ping 192.168.254.15
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.254.15, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R18#ping FD00:3CD5:1001:0:192:168:254:14
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to FD00:3CD5:1001:0:192:168:254:14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/2 ms
R18#ping FD00:3CD5:1001:0:192:168:254:15
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to FD00:3CD5:1001:0:192:168:254:15, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
```

Таким образом видим, что IP-доступность пограничных роутеров между офисами Москва 
и С.-Петербург установлена