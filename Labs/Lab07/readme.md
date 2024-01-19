# OSPF

----

## Задание:

* Настроить IS-IS в ISP Триада.
* R23 и R25 находятся в зоне 2222.
* R24 находится в зоне 24.
* R26 находится в зоне 26.


### Схема реализации (EVE-NG)
\
![ISIS.png](img%2FISIS.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).

### Настройка ISIS маршутизаторах
#### Маршрутизаторы R23 и R25 - area 2222


R23:
```
interface Ethernet0/1
 no shutdown
 description To_R25
 ip address 172.16.2.1 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:1 link-local
 ipv6 address FD00:3CD5:520:172:16:2:0:1/112
 ipv6 enable
 ipv6 router isis 
!
interface Ethernet0/2
 no shutdown
 description To_R24
 ip address 172.16.2.5 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:5 link-local
 ipv6 address FD00:3CD5:520:172:16:2:4:5/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
router isis
 net 49.2222.0192.1682.5423.00
```

R25:
```
interface Ethernet0/0
 no shutdown
 description To_R23
 ip address 172.16.2.2 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:2 link-local
 ipv6 address FD00:3CD5:520:172:16:2:0:2/112
 ipv6 enable
 ipv6 router isis 
!
interface Ethernet0/2
 no shutdown
 description To_R26
 ip address 172.16.2.14 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:14 link-local
 ipv6 address FD00:3CD5:520:172:16:2:12:14/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
router isis
 net 49.2222.0192.1682.5425.00
```

#### Маршутизатор R24 - area 24


R24:
```
interface Ethernet0/1
 no shutdown
 description To_R26
 ip address 172.16.2.9 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:9 link-local
 ipv6 address FD00:3CD5:520:172:16:2:8:9/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
interface Ethernet0/2
 no shutdown
 description To_R23
 ip address 172.16.2.6 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:6 link-local
 ipv6 address FD00:3CD5:520:172:16:2:4:6/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
router isis
 net 49.0024.0192.1682.5424.00
 is-type level-2-only
```

#### Маршутизатор R26 - area 26


R26:
```
interface Ethernet0/0
 no shutdown
 description To_R24
 ip address 172.16.2.10 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:10 link-local
 ipv6 address FD00:3CD5:520:172:16:2:8:10/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
interface Ethernet0/2
 no shutdown
 description To_R25
 ip address 172.16.2.13 255.255.255.252
 ip router isis 
 ipv6 address FE80::172:16:2:13 link-local
 ipv6 address FD00:3CD5:520:172:16:2:12:13/112
 ipv6 enable
 ipv6 router isis 
 isis circuit-type level-2-only
!
router isis
 net 49.0026.0192.1682.5426.00
```

### Таблицы соседства, LSDB и роуты, полученные по ISIS

R23:

```
R23#show isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/2       172.16.2.6      UP    8        R24.02                                                                                         
R25            L1   Et0/1       172.16.2.2      UP    9        R25.01                                                                                         
R25            L2   Et0/1       172.16.2.2      UP    7        R25.01                                                                                         

R23#show isis database

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00           * 0x0000000E   0xE254        482               1/0/0
R25.00-00             0x0000000D   0x41F0        1165              1/0/0
R25.01-00             0x00000005   0x6DF5        1047              0/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R26.00-00             0x00000003   0x8094        0 (523)           0/0/0
R23.00-00           * 0x00000010   0x05D6        361               0/0/0
R24.00-00             0x0000000B   0xD187        1154              0/0/0
R24.02-00             0x00000004   0xF1FB        679               0/0/0
R25.00-00             0x0000000F   0xDBBE        462               0/0/0
R25.01-00             0x00000005   0xFCEE        1073              0/0/0
R26.00-00             0x00000008   0xB368        932               0/0/0
R26.01-00             0x00000005   0x1CCC        967               0/0/0
R26.02-00             0x00000004   0x30B7        1008              0/0/0

R23#show ip route isis

      172.16.0.0/16 is variably subnetted, 6 subnets, 2 masks
i L2     172.16.2.8/30 [115/20] via 172.16.2.6, 00:34:47, Ethernet0/2
i L2     172.16.2.12/30 [115/20] via 172.16.2.2, 00:26:37, Ethernet0/1
```

R24:
```
R24#show isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L2   Et0/2       172.16.2.5      UP    24       R24.02                                                                                         
R26            L2   Et0/1       172.16.2.10     UP    8        R26.01                                                                                         

R24#show isis database

IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R26.00-00             0x00000003   0x8094        0 (523)           0/0/0
R23.00-00             0x00000010   0x05D6        359               0/0/0
R24.00-00           * 0x0000000B   0xD187        1156              0/0/0
R24.02-00           * 0x00000004   0xF1FB        681               0/0/0
R25.00-00             0x0000000F   0xDBBE        460               0/0/0
R25.01-00             0x00000005   0xFCEE        1071              0/0/0
R26.00-00             0x00000008   0xB368        934               0/0/0
R26.01-00             0x00000005   0x1CCC        969               0/0/0
R26.02-00             0x00000004   0x30B7        1010              0/0/0

R24#show ip route isis

      172.16.0.0/16 is variably subnetted, 6 subnets, 2 masks
i L2     172.16.2.0/30 [115/20] via 172.16.2.5, 00:35:06, Ethernet0/2
i L2     172.16.2.12/30 [115/20] via 172.16.2.10, 00:29:55, Ethernet0/1
```

R25:
```
R25#show isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L1   Et0/0       172.16.2.1      UP    26       R25.01                                                                                         
R23            L2   Et0/0       172.16.2.1      UP    24       R25.01                                                                                         
R26            L2   Et0/2       172.16.2.13     UP    9        R26.02                                                                                         

R25#show isis database

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x0000000E   0xE254        480               1/0/0
R25.00-00           * 0x0000000D   0x41F0        1167              1/0/0
R25.01-00           * 0x00000005   0x6DF5        1049              0/0/0
R26.00-00             0x00000001   0xE864        0 (467)           1/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x00000010   0x05D6        359               0/0/0
R24.00-00             0x0000000B   0xD187        1152              0/0/0
R24.02-00             0x00000004   0xF1FB        677               0/0/0
R25.00-00           * 0x0000000F   0xDBBE        464               0/0/0
R25.01-00           * 0x00000005   0xFCEE        1075              0/0/0
R26.00-00             0x00000008   0xB368        934               0/0/0
R26.01-00             0x00000005   0x1CCC        969               0/0/0
R26.02-00             0x00000004   0x30B7        1010              0/0/0

R25#show ip route isis

      172.16.0.0/16 is variably subnetted, 8 subnets, 3 masks
i L2     172.16.2.4/30 [115/20] via 172.16.2.1, 00:26:37, Ethernet0/0
i L2     172.16.2.8/30 [115/20] via 172.16.2.13, 00:30:16, Ethernet0/2
```

R26:
```
R26#show isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/0       172.16.2.9      UP    29       R26.01                                                                                         
R25            L2   Et0/2       172.16.2.14     UP    25       R26.02                                                                                         

R26#show isis database

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R26.00-00           * 0x00000008   0x4F56        890               1/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
0168.0254.0025.00-00  0x00000003   0x8094        0 (523)           0/0/0
R23.00-00             0x00000010   0x05D6        357               0/0/0
R24.00-00             0x0000000B   0xD187        1154              0/0/0
R24.02-00             0x00000004   0xF1FB        679               0/0/0
R25.00-00             0x0000000F   0xDBBE        462               0/0/0
R25.01-00             0x00000005   0xFCEE        1073              0/0/0
R26.00-00           * 0x00000008   0xB368        936               0/0/0
R26.01-00           * 0x00000005   0x1CCC        971               0/0/0
R26.02-00           * 0x00000004   0x30B7        1012              0/0/0

R26#show ip route isis

      172.16.0.0/16 is variably subnetted, 8 subnets, 3 masks
i L2     172.16.2.0/30 [115/20] via 172.16.2.14, 00:30:00, Ethernet0/2
i L2     172.16.2.4/30 [115/20] via 172.16.2.9, 00:30:00, Ethernet0/0
```

Проверка доступности на примере маршрутизатора R23:
```
R23#ping 172.16.2.9
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.9, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R23#ping 172.16.2.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R23#ping 172.16.2.13
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.13, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R23#ping 172.16.2.14
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
