# EIGRP

----

## Задание:

* В офисе С.-Петербург настроить EIGRP.
* R32 получает только маршрут по умолчанию.
* R16-17 анонсируют только суммарные префиксы.
* Использовать EIGRP named-mode для настройки сети.


### Схема реализации (EVE-NG)
\
![EIGRP_NET.png](img%2FEIGRP_NET.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).

### План адресации IPv4/IPv6 для офиса в С.-Петербурге

|                 |           |                  |                |     AS2042      |                                |               |                      |               |
|:----------------|:----------|:-----------------|:--------------:|:---------------:|:------------------------------:|:-------------:|:--------------------:|:-------------:|
|                 |           |                  |                | Санкт-Петербург |                                |               |                      |               |
| Hostname        | Interface | Description      |  IPv4-address  |      Mask       |            IPv6 GUA            | Prefix length |       IPv6 LLA       | Prefix length |
| SW9             | Gi0/3      | To_R17           |  172.16.3.10   |       /30       |  FD00:3CD5:2042:172:16:3:8:10  |     /112      |  fe80::172:16:3:10   | /10           |
|                 | Gi1/0      | To_R16           |  172.16.3.18   |       /30       | FD00:3CD5:2042:172:16:3:16:18  |     /112      |  fe80::172:16:3:18   | /10           |
|                 | Po1       | To_SW10          |  172.16.3.29   |       /30       | FD00:3CD5:2042:172:16:3:28:29  |     /112      |  fe80::172:16:3:29   | /10           |
|                 | VLAN  9   | Gateway_VLAN _9  |    10.0.9.1    |       /30       |    2001:DB8:2042:10::9:0:1     |      /96      |    fe80::10:0:9:1    | /10           |
|                 | Lo1       | Management       | 192.168.254.9  |       /32       | FD00:3CD5:2042::192:168:254:9  |     /128      | fe80::192:168:254:9  | /10           |
| SW10            | Gi0/3      | To_R16           |  172.16.3.22   |       /30       | FD00:3CD5:2042:172:16:3:20:22  |     /112      |  fe80::172:16:3:22   | /10           |
|                 | Gi1/0      | To_R17           |  172.16.3.14   |       /30       | FD00:3CD5:2042:172:16:3:12:14  |     /112      |  fe80::172:16:3:14   | /10           |
|                 | Po1       | To_SW9           |  172.16.3.30   |       /30       | FD00:3CD5:2042:172:16:3:28:30  |     /112      |  fe80::172:16:3:30   | /10           |
|                 | VLAN 10   | Gateway_VLAN _10 |   10.0.10.1    |       /30       |    2001:DB8:2042:10::10:0:1    |      /96      |   fe80::10:0:10:1    | /10           |
|                 | Lo1       | Management       | 192.168.254.10 |       /32       | FD00:3CD5:2042::192:168:254:10 |     /128      | fe80::192:168:254:10 | /10           |
| R16             | e0/0      | To_SW10          |  172.16.3.21   |       /30       | FD00:3CD5:2042:172:16:3:20:21  |     /112      |  fe80::172:16:3:21   | /10           |
|                 | e0/1      | To_R18           |   172.16.3.2   |       /30       |   FD00:3CD5:2042:172:16:3::2   |     /112      |   fe80::172:16:3:2   | /10           |
|                 | e0/2      | To_SW9           |  172.16.3.17   |       /30       | FD00:3CD5:2042:172:16:3:16:17  |     /112      |  fe80::172:16:3:17   | /10           |
|                 | e0/3      | To_R32           |  172.16.3.25   |       /30       | FD00:3CD5:2042:172:16:3:24:25  |     /112      |  fe80::172:16:3:25   | /10           |
|                 | Lo1       | Lo1              | 192.168.254.16 |       /32       | FD00:3CD5:2042::192:168:254:16 |     /128      | fe80::192:168:254:16 | /10           |
| R17             | e0/0      | To_SW9           |   172.16.3.9   |       /30       |  FD00:3CD5:2042:172:16:3:8:9   |     /112      |   fe80::172:16:3:9   | /10           |
|                 | e0/1      | To_R18           |   172.16.3.6   |       /30       |  FD00:3CD5:2042:172:16:3:4:6   |     /112      |   fe80::172:16:3:6   | /10           |
|                 | e0/2      | To_SW10          |  172.16.3.13   |       /30       | FD00:3CD5:2042:172:16:3:12:13  |     /112      |  fe80::172:16:3:13   | /10           |
|                 | Lo1       | Lo1              | 192.168.254.17 |       /32       | FD00:3CD5:2042::192:168:254:17 |     /128      | fe80::192:168:254:17 | /10           |
| R18             | e0/0      | To_R16           |   172.16.3.1   |       /30       |   FD00:3CD5:2042:172:16:3::1   |     /112      |   fe80::172:16:3:1   | /10           |
|                 | e0/1      | To_R17           |   172.16.3.5   |       /30       |  FD00:3CD5:2042:172:16:3:4:5   |     /112      |   fe80::172:16:3:5   | /10           |
|                 | e0/2      | To_R24_AS520     |   20.42.0.1    |       /30       |     2001:DB8:2042:20:42::1     |     /112      |   fe80::20:42:0:1    | /10           |
|                 | e0/3      | To_R26_AS520     |   20.42.0.5    |       /30       |    2001:DB8:2042:20:42::4:5    |     /112      |   fe80::20:42:0:5    | /10           |
|                 | Lo1       | Lo1              | 192.168.254.18 |       /32       | FD00:3CD5:2042::192:168:254:18 |     /128      | fe80::192:168:254:18 | /10           |
| R32             | e0/0      | To_R16           |  172.16.3.26   |       /30       | FD00:3CD5:2042:172:16:3:24:26  |     /112      |  fe80::172:16:3:26   | /10           |
|                 | Lo1       | Lo1              | 192.168.254.32 |       /32       | FD00:3CD5:2042::192:168:254:32 |     /128      | fe80::192:168:254:32 | /10           |

### Настройка EIGRP Named IPv4/IPv6

Маршрутизатор R18 является пограничным, следовательно он должен анонсировать маршрут по-умолчанию
нижестоящим устройствам. В качестве примера создан дефолтный маршрут на Null0.

R18 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  topology base
   redistribute static metric 100000 1000 255 1 1500
  exit-af-topology
  network 172.16.3.1 0.0.0.0
  network 172.16.3.5 0.0.0.0
  network 192.168.254.18 0.0.0.0
  eigrp router-id 18.18.18.18
 exit-address-family
!
ip route 0.0.0.0 0.0.0.0 Null0
```

R18 (IPv6):
```
router eigrp AS2042
  !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   summary-address ::/0
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address ::/0
   no passive-interface
  exit-af-interface
  !
  topology base
   redistribute static metric 100000 1000 255 1 1500
  exit-af-topology
  eigrp router-id 18.18.18.18
 exit-address-family
!
ipv6 route ::/0 Null0
```

Пример включения процесса EIGRP IPv6 на интерфейсе e0/0 R18:

```
interface Ethernet0/0
 no shutdown
 description To_R16
 ip address 172.16.3.1 255.255.255.252
 ipv6 address FE80::172:16:3:1 link-local
 ipv6 address FD00:3CD5:2042:172:16:3:0:1/112
 ipv6 enable
 ipv6 eigrp 2042
 ```

Маршрутизаторы R16 и R17 анонсируют суммарные маршруты в сторону R18. В качестве правила хорошего тона
 и во избежание петли на уровне L3 суммарные маршруты указаны вручную. С R16 в сторону R32 анонсируется
только маршурт по-умолчанию, реализовано с помощью фильтрации на основании префикс-листов For_R32 и
For_R32v6 соответственно для IPv4 и IPv6 версий.
 
R16 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.0.0.0 255.255.240.0
   summary-address 172.16.3.0 255.255.255.0
   summary-address 192.168.254.0 255.255.255.0
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  topology base
   distribute-list prefix For_R32 out Ethernet0/3
  exit-af-topology
  network 172.16.3.2 0.0.0.0
  network 172.16.3.17 0.0.0.0
  network 172.16.3.21 0.0.0.0
  network 172.16.3.25 0.0.0.0
  network 192.168.254.16 0.0.0.0
  eigrp router-id 16.16.16.16
 exit-address-family
 !
 ```

R16 (IPv6):
```
router eigrp AS2042
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address FD00:3CD5:2042::/96
   summary-address 2001:DB8:2042::/96
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
   distribute-list prefix-list For_R32v6 out Ethernet0/3
  exit-af-topology
  eigrp router-id 16.16.16.16
 exit-address-family
!
```

R17 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.0.0.0 255.255.240.0
   summary-address 172.16.3.0 255.255.255.0
   summary-address 192.168.254.0 255.255.255.0
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 172.16.3.6 0.0.0.0
  network 172.16.3.9 0.0.0.0
  network 172.16.3.13 0.0.0.0
  network 192.168.254.17 0.0.0.0
  eigrp router-id 17.17.17.17
 exit-address-family
```

R17 (IPv6):
```
router eigrp AS2042
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address FD00:3CD5:2042::/96
   summary-address 2001:DB8:2042::/96
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 17.17.17.17
 exit-address-family
```

Маршрутизатор R32 не имеет других маршрутов и включений, кроме линка с R16, целесообразно настроить его
как stub.

R32 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 172.16.3.26 0.0.0.0
  network 192.168.254.26 0.0.0.0
  eigrp router-id 32.32.32.32
  eigrp stub connected summary
 exit-address-family
```

R32 (IPv6):
```
router eigrp AS2042
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 32.32.32.32
  eigrp stub connected summary
 exit-address-family
```

Коммутаторы SW9 и SW10 так же участвую в процессе EIGRP и помимо сетей P2P,анонсирую клиентские сети.


SW9 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.0.9.1 0.0.0.0
  network 172.16.3.10 0.0.0.0
  network 172.16.3.18 0.0.0.0
  network 172.16.3.29 0.0.0.0
  network 192.168.254.9 0.0.0.0
  eigrp router-id 9.9.9.9
 exit-address-family
```

SW9 (IPv6):
```
router eigrp AS2042
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 9.9.9.9
 exit-address-family
```

SW10 (IPv4):
```
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Vlan10
   no passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.0.10.1 0.0.0.0
  network 172.16.3.14 0.0.0.0
  network 172.16.3.22 0.0.0.0
  network 172.16.3.30 0.0.0.0
  network 192.168.254.10 0.0.0.0
  eigrp router-id 10.10.10.10
 exit-address-family
```

SW10 (IPv6):
```
router eigrp AS2042
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   bandwidth-percent 20
   passive-interface
  exit-af-interface
  !
  af-interface Port-channel1
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet1/0
   no passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/3
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 9.9.9.9
 exit-address-family
```

### Проверка полученных результатов

После выполнения вышеописанных настроек получены следующий таблицы маршрутизации IPv4/IPv6

R18 (IPv4):
```
Gateway of last resort is 0.0.0.0 to network 0.0.0.0

      10.0.0.0/20 is subnetted, 1 subnets
D        10.0.0.0 [90/1541120] via 172.16.3.6, 00:40:48, Ethernet0/1
                  [90/1541120] via 172.16.3.2, 00:40:48, Ethernet0/0
      172.16.0.0/16 is variably subnetted, 5 subnets, 3 masks
D        172.16.3.0/24 [90/1536000] via 172.16.3.6, 00:40:48, Ethernet0/1
                       [90/1536000] via 172.16.3.2, 00:40:48, Ethernet0/0
      192.168.254.0/24 is variably subnetted, 2 subnets, 2 masks
D        192.168.254.0/24 [90/1024640] via 172.16.3.6, 00:40:48, Ethernet0/1
                          [90/1024640] via 172.16.3.2, 00:40:48, Ethernet0/0
```

R18 (IPv6):
```
D   2001:DB8:2042:10:0:9::/96 [90/1541120]
     via FE80::172:16:3:2, Ethernet0/0
     via FE80::172:16:3:6, Ethernet0/1
D   2001:DB8:2042:10:0:10::/96 [90/1541120]
     via FE80::172:16:3:2, Ethernet0/0
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:0:192:168:254:9/128 [90/1536640]
     via FE80::172:16:3:2, Ethernet0/0
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:0:192:168:254:10/128 [90/1536640]
     via FE80::172:16:3:2, Ethernet0/0
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:0:192:168:254:16/128 [90/1024640]
     via FE80::172:16:3:2, Ethernet0/0
D   FD00:3CD5:2042:0:192:168:254:17/128 [90/1024640]
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:0:192:168:254:32/128 [90/1536640]
     via FE80::172:16:3:2, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:8:0/112 [90/1536000]
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:172:16:3:12:0/112 [90/1536000]
     via FE80::172:16:3:6, Ethernet0/1
D   FD00:3CD5:2042:172:16:3:16:0/112 [90/1536000]
     via FE80::172:16:3:2, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:20:0/112 [90/1536000]
     via FE80::172:16:3:2, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:24:0/112 [90/1536000]
     via FE80::172:16:3:2, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:28:0/112 [90/1538560]
     via FE80::172:16:3:2, Ethernet0/0
     via FE80::172:16:3:6, Ethernet0/1
```

R17 (IPv4):
```
Gateway of last resort is 172.16.3.5 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/6144000] via 172.16.3.5, 00:40:47, Ethernet0/1
      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
D        10.0.0.0/20 is a summary, 17:15:18, Null0
D        10.0.9.0/24 [90/1029120] via 172.16.3.10, 17:15:14, Ethernet0/0
D        10.0.10.0/24 [90/1029120] via 172.16.3.14, 17:15:10, Ethernet0/2
      172.16.0.0/16 is variably subnetted, 12 subnets, 3 masks
D        172.16.3.0/24 is a summary, 17:32:45, Null0
D        172.16.3.0/30 [90/1536000] via 172.16.3.5, 00:40:47, Ethernet0/1
D        172.16.3.16/30 [90/1029120] via 172.16.3.10, 17:15:14, Ethernet0/0
D        172.16.3.20/30 [90/1029120] via 172.16.3.14, 17:15:14, Ethernet0/2
D        172.16.3.24/30 [90/1541120] via 172.16.3.14, 17:15:15, Ethernet0/2
                        [90/1541120] via 172.16.3.10, 17:15:15, Ethernet0/0
D        172.16.3.28/30 [90/1026560] via 172.16.3.14, 17:15:15, Ethernet0/2
                        [90/1026560] via 172.16.3.10, 17:15:15, Ethernet0/0
      192.168.254.0/24 is variably subnetted, 6 subnets, 2 masks
D        192.168.254.0/24 is a summary, 17:32:45, Null0
D        192.168.254.9/32 [90/1024640] via 172.16.3.10, 17:15:14, Ethernet0/0
D        192.168.254.10/32 [90/1024640] via 172.16.3.14, 17:15:14, Ethernet0/2
D        192.168.254.16/32 [90/1029760] via 172.16.3.14, 17:15:15, Ethernet0/2
                           [90/1029760] via 172.16.3.10, 17:15:15, Ethernet0/0
D        192.168.254.18/32 [90/1024640] via 172.16.3.5, 00:40:47, Ethernet0/1
```

R17 (IPv6):
```
EX  ::/0 [170/6144000]
     via FE80::172:16:3:5, Ethernet0/1
D   2001:DB8:2042:10:0:9::/96 [90/1029120]
     via FE80::172:16:3:10, Ethernet0/0
D   2001:DB8:2042:10:0:10::/96 [90/1029120]
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:0:192:168:254:9/128 [90/1024640]
     via FE80::172:16:3:10, Ethernet0/0
D   FD00:3CD5:2042:0:192:168:254:10/128 [90/1024640]
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:0:192:168:254:16/128 [90/1029760]
     via FE80::172:16:3:10, Ethernet0/0
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:0:192:168:254:32/128 [90/1541760]
     via FE80::172:16:3:10, Ethernet0/0
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:172:16:3::/112 [90/1541120]
     via FE80::172:16:3:10, Ethernet0/0
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:172:16:3:16:0/112 [90/1029120]
     via FE80::172:16:3:10, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:20:0/112 [90/1029120]
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:172:16:3:24:0/112 [90/1541120]
     via FE80::172:16:3:10, Ethernet0/0
     via FE80::172:16:3:14, Ethernet0/2
D   FD00:3CD5:2042:172:16:3:28:0/112 [90/1026560]
     via FE80::172:16:3:10, Ethernet0/0
     via FE80::172:16:3:14, Ethernet0/2
```

R16 (IPv4):
```
Gateway of last resort is 172.16.3.1 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/6144000] via 172.16.3.1, 00:40:47, Ethernet0/1
      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
D        10.0.0.0/20 is a summary, 17:15:18, Null0
D        10.0.9.0/24 [90/1029120] via 172.16.3.18, 17:15:14, Ethernet0/2
D        10.0.10.0/24 [90/1029120] via 172.16.3.22, 17:15:10, Ethernet0/0
      172.16.0.0/16 is variably subnetted, 13 subnets, 3 masks
D        172.16.3.0/24 is a summary, 17:32:44, Null0
D        172.16.3.4/30 [90/1536000] via 172.16.3.1, 00:40:47, Ethernet0/1
D        172.16.3.8/30 [90/1029120] via 172.16.3.18, 17:15:14, Ethernet0/2
D        172.16.3.12/30 [90/1029120] via 172.16.3.22, 17:15:14, Ethernet0/0
D        172.16.3.28/30 [90/1026560] via 172.16.3.22, 17:15:15, Ethernet0/0
                        [90/1026560] via 172.16.3.18, 17:15:15, Ethernet0/2
      192.168.254.0/24 is variably subnetted, 6 subnets, 2 masks
D        192.168.254.0/24 is a summary, 17:32:44, Null0
D        192.168.254.9/32 [90/1024640] via 172.16.3.18, 17:15:14, Ethernet0/2
D        192.168.254.10/32 [90/1024640] via 172.16.3.22, 17:15:14, Ethernet0/0
D        192.168.254.17/32 [90/1029760] via 172.16.3.22, 17:15:15, Ethernet0/0
                           [90/1029760] via 172.16.3.18, 17:15:15, Ethernet0/2
D        192.168.254.18/32 [90/1024640] via 172.16.3.1, 00:40:47, Ethernet0/1
```

R16 (IPv6):
```
EX  ::/0 [170/6144000]
     via FE80::172:16:3:1, Ethernet0/1
D   2001:DB8:2042:10:0:9::/96 [90/1029120]
     via FE80::172:16:3:18, Ethernet0/2
D   2001:DB8:2042:10:0:10::/96 [90/1029120]
     via FE80::172:16:3:22, Ethernet0/0
D   FD00:3CD5:2042:0:192:168:254:9/128 [90/1024640]
     via FE80::172:16:3:18, Ethernet0/2
D   FD00:3CD5:2042:0:192:168:254:10/128 [90/1024640]
     via FE80::172:16:3:22, Ethernet0/0
D   FD00:3CD5:2042:0:192:168:254:17/128 [90/1029760]
     via FE80::172:16:3:18, Ethernet0/2
     via FE80::172:16:3:22, Ethernet0/0
D   FD00:3CD5:2042:0:192:168:254:32/128 [90/1024640]
     via FE80::172:16:3:26, Ethernet0/3
D   FD00:3CD5:2042:172:16:3:4:0/112 [90/1541120]
     via FE80::172:16:3:18, Ethernet0/2
     via FE80::172:16:3:22, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:8:0/112 [90/1029120]
     via FE80::172:16:3:18, Ethernet0/2
D   FD00:3CD5:2042:172:16:3:12:0/112 [90/1029120]
     via FE80::172:16:3:22, Ethernet0/0
D   FD00:3CD5:2042:172:16:3:28:0/112 [90/1026560]
     via FE80::172:16:3:18, Ethernet0/2
     via FE80::172:16:3:22, Ethernet0/0
```

SW10 (IPv4):
```
Gateway of last resort is 172.16.3.21 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/6149120] via 172.16.3.21, 00:40:22, GigabitEthernet0/3
                [170/6149120] via 172.16.3.13, 00:40:22, GigabitEthernet1/0
      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
D        10.0.9.0/24 [90/12800] via 172.16.3.29, 16:55:22, Port-channel1
      172.16.0.0/16 is variably subnetted, 11 subnets, 2 masks
D        172.16.3.0/30
           [90/1029120] via 172.16.3.21, 00:40:22, GigabitEthernet0/3
D        172.16.3.4/30
           [90/1029120] via 172.16.3.13, 00:40:22, GigabitEthernet1/0
D        172.16.3.8/30 [90/12800] via 172.16.3.29, 16:55:22, Port-channel1
D        172.16.3.16/30 [90/12800] via 172.16.3.29, 16:55:22, Port-channel1
D        172.16.3.24/30
           [90/1029120] via 172.16.3.21, 16:55:20, GigabitEthernet0/3
      192.168.254.0/32 is subnetted, 5 subnets
D        192.168.254.9 [90/5760] via 172.16.3.29, 16:55:22, Port-channel1
D        192.168.254.16
           [90/10880] via 172.16.3.21, 16:55:20, GigabitEthernet0/3
D        192.168.254.17
           [90/10880] via 172.16.3.13, 16:55:20, GigabitEthernet1/0
D        192.168.254.18
           [90/1029760] via 172.16.3.21, 00:40:22, GigabitEthernet0/3
           [90/1029760] via 172.16.3.13, 00:40:22, GigabitEthernet1/0
```

SW10 (IPv6):
```
EX  ::/0 [170/6149120]
     via FE80::172:16:3:21, GigabitEthernet0/3
     via FE80::172:16:3:13, GigabitEthernet1/0
D   FD00:3CD5:2042:0:192:168:254:16/128 [90/10880]
     via FE80::172:16:3:21, GigabitEthernet0/3
D   FD00:3CD5:2042:0:192:168:254:17/128 [90/10880]
     via FE80::172:16:3:13, GigabitEthernet1/0
D   FD00:3CD5:2042:0:192:168:254:32/128 [90/1029760]
     via FE80::172:16:3:21, GigabitEthernet0/3
D   FD00:3CD5:2042:172:16:3::/112 [90/1029120]
     via FE80::172:16:3:21, GigabitEthernet0/3
D   FD00:3CD5:2042:172:16:3:4:0/112 [90/1029120]
     via FE80::172:16:3:13, GigabitEthernet1/0
D   FD00:3CD5:2042:172:16:3:8:0/112 [90/1029120]
     via FE80::172:16:3:13, GigabitEthernet1/0
D   FD00:3CD5:2042:172:16:3:16:0/112 [90/1029120]
     via FE80::172:16:3:21, GigabitEthernet0/3
D   FD00:3CD5:2042:172:16:3:24:0/112 [90/1029120]
     via FE80::172:16:3:21, GigabitEthernet0/3
```

SW9 (IPv4):
```
Gateway of last resort is 172.16.3.17 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/6149120] via 172.16.3.17, 00:00:08, GigabitEthernet1/0
                [170/6149120] via 172.16.3.9, 00:00:08, GigabitEthernet0/3
      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
D        10.0.10.0/24 [90/12800] via 172.16.3.30, 16:57:08, Port-channel1
      172.16.0.0/16 is variably subnetted, 11 subnets, 2 masks
D        172.16.3.0/30
           [90/1029120] via 172.16.3.17, 00:40:40, GigabitEthernet1/0
D        172.16.3.4/30
           [90/1029120] via 172.16.3.9, 00:40:40, GigabitEthernet0/3
D        172.16.3.12/30 [90/12800] via 172.16.3.30, 16:57:11, Port-channel1
D        172.16.3.20/30 [90/12800] via 172.16.3.30, 16:57:11, Port-channel1
D        172.16.3.24/30
           [90/1029120] via 172.16.3.17, 16:57:08, GigabitEthernet1/0
      192.168.254.0/32 is subnetted, 5 subnets
D        192.168.254.10 [90/5760] via 172.16.3.30, 16:57:11, Port-channel1
D        192.168.254.16
           [90/10880] via 172.16.3.17, 16:57:08, GigabitEthernet1/0
D        192.168.254.17
           [90/10880] via 172.16.3.9, 16:57:08, GigabitEthernet0/3
D        192.168.254.18
           [90/1029760] via 172.16.3.17, 00:40:40, GigabitEthernet1/0
           [90/1029760] via 172.16.3.9, 00:40:40, GigabitEthernet0/3
```

SW9 (IPv6):
```
EX  ::/0 [170/6149120]
     via FE80::172:16:3:17, GigabitEthernet1/0
     via FE80::172:16:3:9, GigabitEthernet0/3
D   FD00:3CD5:2042:0:192:168:254:16/128 [90/10880]
     via FE80::172:16:3:17, GigabitEthernet1/0
D   FD00:3CD5:2042:0:192:168:254:17/128 [90/10880]
     via FE80::172:16:3:9, GigabitEthernet0/3
D   FD00:3CD5:2042:0:192:168:254:32/128 [90/1029760]
     via FE80::172:16:3:17, GigabitEthernet1/0
D   FD00:3CD5:2042:172:16:3::/112 [90/1029120]
     via FE80::172:16:3:17, GigabitEthernet1/0
D   FD00:3CD5:2042:172:16:3:4:0/112 [90/1029120]
     via FE80::172:16:3:9, GigabitEthernet0/3
D   FD00:3CD5:2042:172:16:3:12:0/112 [90/1029120]
     via FE80::172:16:3:9, GigabitEthernet0/3
D   FD00:3CD5:2042:172:16:3:20:0/112 [90/1029120]
     via FE80::172:16:3:17, GigabitEthernet1/0
D   FD00:3CD5:2042:172:16:3:24:0/112 [90/1029120]
     via FE80::172:16:3:17, GigabitEthernet1/0
```

В связи с ограниченными возможностями VPCS для проверки по ICMP использован хост Ubuntu с двумя включениями - 
в SW9 (Gi1/1) и SW10 (Gi1/1).
Результат проверки по ICMP, MTR:
```
root@ubuntu:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:04:00 brd ff:ff:ff:ff:ff:ff
    inet 10.0.9.3/24 brd 10.0.9.255 scope global ens3
       valid_lft forever preferred_lft forever
    inet6 2001:db8:2042:10:0:9:ac48:9d8/128 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:400/64 scope link
       valid_lft forever preferred_lft forever
3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:04:01 brd ff:ff:ff:ff:ff:ff
    inet 10.0.10.3/24 brd 10.0.10.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 2001:db8:2042:10:0:10:d454:95a2/128 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:401/64 scope link
       valid_lft forever preferred_lft forever

root@ubuntu:~# ping 172.16.3.1
PING 172.16.3.1 (172.16.3.1) 56(84) bytes of data.
64 bytes from 172.16.3.1: icmp_seq=1 ttl=253 time=6.57 ms
64 bytes from 172.16.3.1: icmp_seq=2 ttl=253 time=7.76 ms
64 bytes from 172.16.3.1: icmp_seq=3 ttl=253 time=6.31 ms
^C
--- 172.16.3.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 6.318/6.882/7.760/0.636 ms

root@ubuntu:~# ping6 FD00:3CD5:2042::192:168:254:32
PING FD00:3CD5:2042::192:168:254:32(fd00:3cd5:2042:0:192:168:254:32) 56 data bytes
64 bytes from fd00:3cd5:2042:0:192:168:254:32: icmp_seq=1 ttl=62 time=6.72 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:32: icmp_seq=2 ttl=62 time=8.09 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:32: icmp_seq=3 ttl=62 time=8.30 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:32: icmp_seq=4 ttl=62 time=4.44 ms
^C
--- FD00:3CD5:2042::192:168:254:32 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 4.445/6.893/8.309/1.542 ms

root@ubuntu:~# ping6 FD00:3CD5:2042::192:168:254:18
PING FD00:3CD5:2042::192:168:254:18(fd00:3cd5:2042:0:192:168:254:18) 56 data bytes
64 bytes from fd00:3cd5:2042:0:192:168:254:18: icmp_seq=1 ttl=62 time=4.74 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:18: icmp_seq=2 ttl=62 time=7.91 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:18: icmp_seq=3 ttl=62 time=11.3 ms
64 bytes from fd00:3cd5:2042:0:192:168:254:18: icmp_seq=4 ttl=62 time=7.59 ms
^C
--- FD00:3CD5:2042::192:168:254:18 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 4.745/7.900/11.349/2.343 ms

root@ubuntu:~# ping 192.168.254.16
PING 192.168.254.16 (192.168.254.16) 56(84) bytes of data.
64 bytes from 192.168.254.16: icmp_seq=1 ttl=254 time=4.18 ms
64 bytes from 192.168.254.16: icmp_seq=2 ttl=254 time=5.32 ms
64 bytes from 192.168.254.16: icmp_seq=3 ttl=254 time=4.95 ms
64 bytes from 192.168.254.16: icmp_seq=4 ttl=254 time=3.71 ms
^C
--- 192.168.254.16 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 3.717/4.546/5.324/0.636 ms

root@ubuntu:~# ping 192.168.254.9
PING 192.168.254.9 (192.168.254.9) 56(84) bytes of data.
64 bytes from 192.168.254.9: icmp_seq=1 ttl=254 time=17.5 ms
64 bytes from 192.168.254.9: icmp_seq=2 ttl=254 time=22.7 ms
64 bytes from 192.168.254.9: icmp_seq=3 ttl=254 time=28.3 ms
64 bytes from 192.168.254.9: icmp_seq=4 ttl=254 time=15.4 ms
^C
--- 192.168.254.9 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 15.473/21.050/28.354/4.989 ms

                             My traceroute  [v0.86]
ubuntu (0.0.0.0)                                       Mon Jan 29 13:53:57 2024
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 10.0.10.1                         0.0%     5    6.6   7.4   6.2   9.8   1.2
 2. 172.16.3.29                       0.0%     5   13.5  20.1  13.5  28.2   6.1
 3. 172.16.3.9                        0.0%     5   11.6  12.4  11.2  14.1   0.9

                             My traceroute  [v0.86]
ubuntu (0.0.0.0)                                       Mon Jan 29 14:02:05 2024
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 10.0.10.1                         0.0%    12    8.9   9.8   6.5  13.6   2.4
 2. 172.16.3.21                       0.0%    12    8.4   8.2   4.6  15.9   2.7
 3. 192.168.254.32                    0.0%    12    6.8   7.0   3.4   9.3   1.6

                             My traceroute  [v0.86]
ubuntu (::)                                            Mon Jan 29 14:02:43 2024
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 2001:db8:2042:10:0:10:0:1         0.0%     5    6.8   7.7   4.6  11.3   2.4
 2. fd00:3cd5:2042:172:16:3:12:13     0.0%     4    3.1   6.4   3.1   7.8   2.2
 3. fd00:3cd5:2042:0:192:168:254:18   0.0%     4    4.8   5.5   4.8   6.4   0.0
```

Как видим с хоста Ubuntu видны все сети данного офиса, чего и требовалось достичь в данной
лабораторной работе.