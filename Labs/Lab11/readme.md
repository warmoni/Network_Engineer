# iBGP

----

## Задание:

* Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
* Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
* Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
* Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
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


### Настройка фильрации в офисе Москва

Для того чтоб исключить возможность использования автономной системы 1001 офиса Москва
в качестве транзитной, необходимо из AS1001 анонсить только свои сети. Для этого используем
filter-list в направлении соседей eBGP (AS101, AS301).

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
  neighbor 100.1.0.2 filter-list 1 out
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
  neighbor 2001:DB8:1001:100:1::2 filter-list 1 out
  neighbor FD00:3CD5:1001:0:192:168:254:15 activate
  neighbor FD00:3CD5:1001:0:192:168:254:15 soft-reconfiguration inbound
!
ip as-path access-list 1 permit ^$
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
  neighbor 100.1.0.6 filter-list 1 out
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
  neighbor 2001:DB8:1001:100:1:0:4:6 filter-list 1 out
  neighbor FD00:3CD5:1001:0:192:168:254:12 activate
  neighbor FD00:3CD5:1001:0:192:168:254:12 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:12 soft-reconfiguration inbound
  neighbor FD00:3CD5:1001:0:192:168:254:13 activate
  neighbor FD00:3CD5:1001:0:192:168:254:13 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:13 soft-reconfiguration inbound
  neighbor FD00:3CD5:1001:0:192:168:254:14 activate
  neighbor FD00:3CD5:1001:0:192:168:254:14 route-reflector-client
  neighbor FD00:3CD5:1001:0:192:168:254:14 soft-reconfiguration inbound
 exit-address-family
!
ip as-path access-list 1 permit ^$
```

BGP-таблица на eBGP-соседях:

R21:
```
IPv4
R21#sh ip bgp neighbors 100.1.0.5 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   100.1.0.0/30     100.1.0.5                              0 1001 i
 *   100.1.0.4/30     100.1.0.5                0             0 1001 i
 *>  192.168.254.14/32
                       100.1.0.5                              0 1001 i
 *>  192.168.254.15/32
                       100.1.0.5                0             0 1001 i

Total number of prefixes 4

IPv6
R21#sh ip bgp ipv6 uni neighbors 2001:DB8:1001:100:1:0:4:5 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   2001:DB8:1001:100:1::/112
                       2001:DB8:1001:100:1:0:4:5
                                                              0 1001 i
 *   2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:1001:100:1:0:4:5
                                                0             0 1001 i
 *>  FD00:3CD5:1001:0:192:168:254:14/128
                       2001:DB8:1001:100:1:0:4:5
                                                              0 1001 i
 *>  FD00:3CD5:1001:0:192:168:254:15/128
                       2001:DB8:1001:100:1:0:4:5
                                                0             0 1001 i

Total number of prefixes 4
```


R22:
```
IPv4
R22#sh ip bgp neighbors 100.1.0.1 received-routes
     Network          Next Hop            Metric LocPrf Weight Path
 *   100.1.0.0/30     100.1.0.1                0             0 1001 1001 1001 1001 i
 *   100.1.0.4/30     100.1.0.1                              0 1001 1001 1001 1001 i
 *   192.168.254.14/32
                       100.1.0.1                0             0 1001 1001 1001 1001 i
 *   192.168.254.15/32
                       100.1.0.1                              0 1001 1001 1001 1001 i

Total number of prefixes 4

IPv6
R22#sh ip bgp ipv6 uni neighbors 2001:DB8:1001:100:1:0::1 received-routes
     Network          Next Hop            Metric LocPrf Weight Path
 *   2001:DB8:1001:100:1::/112
                       2001:DB8:1001:100:1::1
                                                0             0 1001 1001 1001 1001 i
 *   2001:DB8:1001:100:1:0:4:0/112
                       2001:DB8:1001:100:1::1
                                                              0 1001 1001 1001 1001 i
 *   FD00:3CD5:1001:0:192:168:254:14/128
                       2001:DB8:1001:100:1::1
                                                0             0 1001 1001 1001 1001 i
 *   FD00:3CD5:1001:0:192:168:254:15/128
                       2001:DB8:1001:100:1::1
     Network          Next Hop            Metric LocPrf Weight Path
                                                              0 1001 1001 1001 1001 i

Total number of prefixes 4
```

### Настройка фильтрации в офисе С.-Петербург


В автономной системе 2042 офиса С.Петербург также не должен появится транизитный трафик.
Осуществим фильтрацию с помощью prefix-list, AS2042 должна анонсить сосдем eBGP  только свои сети.


R18
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
  neighbor 20.42.0.2 prefix-list No_Transit_v4 out
  neighbor 20.42.0.6 activate
  neighbor 20.42.0.6 soft-reconfiguration inbound
  neighbor 20.42.0.6 prefix-list No_Transit_v4 out
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
  neighbor 2001:DB8:2042:20:42::2 prefix-list No_Transit_v6 out
  neighbor 2001:DB8:2042:20:42:0:4:6 activate
  neighbor 2001:DB8:2042:20:42:0:4:6 soft-reconfiguration inbound
  neighbor 2001:DB8:2042:20:42:0:4:6 prefix-list No_Transit_v6 out
 exit-address-family
!
ip prefix-list No_Transit_v4 seq 5 permit 20.42.0.0/30
ip prefix-list No_Transit_v4 seq 10 permit 20.42.0.4/30
ip prefix-list No_Transit_v4 seq 15 permit 192.168.254.18/32
!
!
ipv6 prefix-list No_Transit_v6 seq 5 permit 2001:DB8:2042:20:42::/112
ipv6 prefix-list No_Transit_v6 seq 10 permit 2001:DB8:2042:20:42:0:4:0/112
ipv6 prefix-list No_Transit_v6 seq 15 permit FD00:3CD5:2042:0:192:168:254:18/128
```

BGP-таблица на eBGP-соседях:

R24:
```
IPv4
R24#sh ip bgp neighbors 20.42.0.1 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   20.42.0.0/30     20.42.0.1                0             0 2042 i
 *   20.42.0.4/30     20.42.0.1                0             0 2042 i
 *>  192.168.254.18/32
                       20.42.0.1                0             0 2042 i

Total number of prefixes 3

IPv6
R24#sh ip bgp ipv6 uni neighbors 2001:DB8:2042:20:42::1 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   2001:DB8:2042:20:42::/112
                       2001:DB8:2042:20:42::1
                                                0             0 2042 i
 *   2001:DB8:2042:20:42:0:4:0/112
                       2001:DB8:2042:20:42::1
                                                0             0 2042 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:2042:20:42::1
                                                0             0 2042 i

Total number of prefixes 3
```

R26:
```
IPv4
R26#sh ip bgp neighbors 20.42.0.5 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   20.42.0.0/30     20.42.0.5                0             0 2042 i
 *   20.42.0.4/30     20.42.0.5                0             0 2042 i
 *>  192.168.254.18/32
                       20.42.0.5                0             0 2042 i

Total number of prefixes 3

IPv6
R26#sh ip bgp ipv6 uni neighbors 2001:DB8:2042:20:42:0:4:5 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   2001:DB8:2042:20:42::/112
                       2001:DB8:2042:20:42:0:4:5
                                                0             0 2042 i
 *   2001:DB8:2042:20:42:0:4:0/112
                       2001:DB8:2042:20:42:0:4:5
                                                0             0 2042 i
 *>  FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:2042:20:42:0:4:5
                                                0             0 2042 i

Total number of prefixes 3
```

### Настройка провайдера Киторн

Провайдер Киторн должен анонсить в офис Москва только маршрут по-умолчанию. Для этого
воспользуемся route-map и default-originate в сторону AS1001 офиса Москва.

R22:
```
R22#sh run | sec bgp
router bgp 101
 bgp log-neighbor-changes
 neighbor 2001:DB8:101:101::2 remote-as 520
 neighbor 2001:DB8:101:101::4:6 remote-as 301
 neighbor 2001:DB8:1001:100:1::1 remote-as 1001
 neighbor 100.1.0.1 remote-as 1001
 neighbor 101.0.0.2 remote-as 520
 neighbor 101.0.0.6 remote-as 301
 !
 address-family ipv4
  network 100.1.0.0 mask 255.255.255.252
  network 101.0.0.0 mask 255.255.255.252
  network 101.0.0.4 mask 255.255.255.252
  network 192.168.254.22 mask 255.255.255.255
  no neighbor 2001:DB8:101:101::2 activate
  no neighbor 2001:DB8:101:101::4:6 activate
  no neighbor 2001:DB8:1001:100:1::1 activate
  neighbor 100.1.0.1 activate
  neighbor 100.1.0.1 default-originate
  neighbor 100.1.0.1 soft-reconfiguration inbound
  neighbor 100.1.0.1 route-map Default_To_R14_RM out
  neighbor 101.0.0.2 activate
  neighbor 101.0.0.2 soft-reconfiguration inbound
  neighbor 101.0.0.6 activate
  neighbor 101.0.0.6 soft-reconfiguration inbound
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:101:101::/112
  network 2001:DB8:101:101::4:0/112
  network 2001:DB8:1001:100:1::/112
  network FD00:3CD5:101:0:192:168:254:22/128
  neighbor 2001:DB8:101:101::2 activate
  neighbor 2001:DB8:101:101::2 soft-reconfiguration inbound
  neighbor 2001:DB8:101:101::4:6 activate
  neighbor 2001:DB8:101:101::4:6 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1::1 activate
  neighbor 2001:DB8:1001:100:1::1 default-originate
  neighbor 2001:DB8:1001:100:1::1 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1::1 route-map Default_To_R14 out
 exit-address-family
!
ip prefix-list Default_To_R14 seq 5 permit 0.0.0.0/0
!
!
ipv6 prefix-list Default_To_R14v6 seq 5 permit ::/0
route-map Default_To_R14_RM permit 5
 match ip address prefix-list Default_To_R14
 match ipv6 address prefix-list Default_To_R14v6
```

BGP-таблциа на R14:

```
IPv4
R14#sh ip bgp neighbors 100.1.0.2 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 r   0.0.0.0          100.1.0.2                              0 101 i

Total number of prefixes 1

IPv6
R14#sh ip bgp ipv6 uni neighbors  2001:DB8:1001:100:1::2 received-routes


     Network          Next Hop            Metric LocPrf Weight Path
 r   ::/0             2001:DB8:1001:100:1::2
                                                              0 101 i

Total number of prefixes 1
```

### Настройка провайдера Ламас

Провайдер Ламас должен анонсить в офис Москва маршрут по-умолчанию и сети офиса 
С.-Петербург. Для этого воспользуемся filter-list и default-originate в сторону 
AS1001 офиса Москва.


R21:
```
R21#sh run | sec bgp
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
  neighbor 30.1.0.2 soft-reconfiguration inbound
  no neighbor 2001:DB8:101:101::4:5 activate
  no neighbor 2001:DB8:301:30:1::2 activate
  no neighbor 2001:DB8:1001:100:1:0:4:5 activate
  neighbor 100.1.0.5 activate
  neighbor 100.1.0.5 default-originate
  neighbor 100.1.0.5 soft-reconfiguration inbound
  neighbor 100.1.0.5 filter-list 1 out
  neighbor 101.0.0.5 activate
  neighbor 101.0.0.5 soft-reconfiguration inbound
 exit-address-family
 !
 address-family ipv6
  network 2001:DB8:101:101::4:0/112
  network 2001:DB8:301:30:1::/112
  network 2001:DB8:1001:100:1:0:4:0/112
  network FD00:3CD5:301:0:192:168:254:21/128
  neighbor 2001:DB8:101:101::4:5 activate
  neighbor 2001:DB8:101:101::4:5 soft-reconfiguration inbound
  neighbor 2001:DB8:301:30:1::2 activate
  neighbor 2001:DB8:301:30:1::2 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1:0:4:5 activate
  neighbor 2001:DB8:1001:100:1:0:4:5 default-originate
  neighbor 2001:DB8:1001:100:1:0:4:5 soft-reconfiguration inbound
  neighbor 2001:DB8:1001:100:1:0:4:5 filter-list 1 out
 exit-address-family
!
ip as-path access-list 1 permit _2042$
ip as-path access-list 1 deny .*
```

BGP-таблица на R15:
```
IPv4
R15#sh ip bgp neighbors 100.1.0.6 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   0.0.0.0          100.1.0.6                              0 301 i
 *   192.168.254.18/32
                       100.1.0.6                              0 301 520 2042 i

Total number of prefixes 2

IPv6
R15#sh ip bgp ipv6 uni nei 2001:DB8:1001:100:1:0:4:6 received-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *   ::/0             2001:DB8:1001:100:1:0:4:6
                                                              0 301 i
 *   FD00:3CD5:2042:0:192:168:254:18/128
                       2001:DB8:1001:100:1:0:4:6
                                                              0 301 520 2042 i

Total number of prefixes 2
```

### Проверка IP-связности:


Трассировка IPv4 с R14 на R18, R27, R28
```
R14#traceroute 52.0.0.10 source lo1
Type escape sequence to abort.
Tracing the route to 52.0.0.10
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 [AS 301] 1 msec 0 msec 1 msec
  2 100.1.0.6 1 msec 1 msec 1 msec
  3 30.1.0.2 [AS 301] 1 msec 2 msec 2 msec
  4 172.16.2.10 [AS 301] 2 msec 2 msec 2 msec
  5 52.0.0.10 [AS 301] 3 msec *  2 msec
R14#traceroute 192.168.254.18 source lo1
Type escape sequence to abort.
Tracing the route to 192.168.254.18
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 [AS 301] 1 msec 0 msec 1 msec
  2 100.1.0.6 1 msec 1 msec 0 msec
  3 30.1.0.2 [AS 301] 1 msec 1 msec 2 msec
  4 20.42.0.1 [AS 301] 2 msec *  2 msec
R14#traceroute 52.0.0.2 source lo1
Type escape sequence to abort.
Tracing the route to 52.0.0.2
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 [AS 301] 1 msec 1 msec 0 msec
  2 100.1.0.6 1 msec 1 msec 1 msec
  3 30.1.0.2 [AS 301] 1 msec 2 msec 1 msec
  4 172.16.2.10 [AS 301] 2 msec 1 msec 1 msec
  5 172.16.2.14 [AS 301] 2 msec 2 msec 1 msec
  6 52.0.0.2 [AS 301] 3 msec *  3 msec
R14#traceroute 52.0.0.6 source lo1
Type escape sequence to abort.
Tracing the route to 52.0.0.6
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.1.46 [AS 301] 0 msec 1 msec 1 msec
  2 100.1.0.6 1 msec 1 msec 1 msec
  3 30.1.0.2 [AS 301] 1 msec 1 msec 1 msec
  4 172.16.2.5 [AS 301] 2 msec 1 msec 2 msec
  5 172.16.2.2 [AS 301] 2 msec 2 msec 2 msec
  6 52.0.0.6 [AS 301] 2 msec *  3 msec
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

  1 FD00:3CD5:1001:172:16:1:44:46 [AS 301] 1 msec 1 msec 0 msec
  2 2001:DB8:1001:100:1:0:4:6 2 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 1 msec 2 msec 1 msec
  4 2001:DB8:2042:20:42::1 [AS 301] 2 msec 2 msec 2 msec
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

  1 FD00:3CD5:1001:172:16:1:44:46 [AS 301] 1 msec 1 msec 0 msec
  2 2001:DB8:1001:100:1:0:4:6 1 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 1 msec 1 msec 1 msec
  4 FD00:3CD5:520:172:16:2:8:10 [AS 301] 3 msec 2 msec 3 msec
  5 FD00:3CD5:520:172:16:2:12:14 [AS 301] 3 msec 2 msec 2 msec
  6 2001:DB8:520:52::2 [AS 301] 30 msec 3 msec 2 msec
R14#trace
R14#traceroute ipv6
Target IPv6 address: 2001:DB8:520:52::4:6
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

  1 FD00:3CD5:1001:172:16:1:44:46 [AS 301] 1 msec 0 msec 1 msec
  2 2001:DB8:1001:100:1:0:4:6 1 msec 1 msec 1 msec
  3 2001:DB8:301:30:1::2 [AS 301] 1 msec 2 msec 1 msec
  4 FD00:3CD5:520:172:16:2:8:10 [AS 301] 2 msec 2 msec 2 msec
  5 FD00:3CD5:520:172:16:2:12:14 [AS 301] 3 msec 2 msec 2 msec
  6 2001:DB8:520:52::4:6 [AS 301] 39 msec 2 msec 2 msec
```

Фильтрация применена, IP-связность сохранена.