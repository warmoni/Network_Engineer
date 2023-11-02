# Настройка SLAAC, SLAAC+DHCPv6, Stateful DHCPv6

----

## Задание:

* Создание сети и настройка базовых параметров устройств.
* Проверка назначения адреса SLAAC с помощью R1.
* Настройка и проверка Stateless DHCPv6 сервера на маршрутизаторе R1.
* Настройка и проверка Stateful DHCPv6 сервера на маршрутизаторе R1.
* Настройка и проверка DHCPv6 Relay на маршрутизаторе R2.
 


### Схема реализации (EVE-NG)
\
![scheme.png](img%2Fscheme.png)

Таблица распределения адресного пространства 

| **Device** | **Interface** | **IPv6 Address**       |
|------------|---------------|------------------------|
| R1         | G0/0/0        | 2001:db8:acad:2::1 /64 |
| R1         | G0/0/0        | fe80::1                |
| R1         | G0/0/1        | 2001:db8:acad:1::1/64  |
| R1         | G0/0/1        | fe80::1                |
| R2         | G0/0/0        | 2001:db8:acad:2::2/64  |
| R2         | G0/0/0        | fe80::2                |
| R2         | G0/0/1        | 2001:db8:acad:3::1 /64 |
| R2         | G0/0/1        | fe80::1                |
| PC-A       | NIC           | DHCP                   |
| PC-B       | NIC           | DHCP                   |


Настройка сетевых устройств S1, S2, R1, R2, приведена по [ссылке](Configs%2Freadme.md).


### Проверка назначения адреса SLAAC с помощью R1

Настройка интерфейса e0/1 на маршрутизаторе R1 и включение роутинга IPv6:

```
ipv6 unicast-routing
interface Ethernet0/1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 ipv6 enable
```

Проверка назначения IPv6 адреса на интерфейсе ens3 VM_CentOS:

```
[root@localhost ~]# nmcli d show ens3
GENERAL.DEVICE:                         ens3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         00:50:00:00:06:00
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     ens3
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/5
WIRED-PROPERTIES.CARRIER:               on
IP6.ADDRESS[1]:                         2001:db8:acad:1:eb1c:9d54:50c6:4cdc/64
IP6.ADDRESS[2]:                         fe80::7d37:9eb2:fcf0:a45/64
IP6.GATEWAY:                            fe80::1
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 102
IP6.ROUTE[2]:                           dst = 2001:db8:acad:1::/64, nh = ::, mt = 102
IP6.ROUTE[3]:                           dst = ::/0, nh = fe80::1, mt = 102
IP6.ROUTE[4]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
```

Из листинга видно, что хост получил от R1 префикс, длину префикса и сгенерировал GUA.\
Проверка достпуности шлюза с VM_CentOS:

```
[root@localhost ~]# ping6 -I ens3 2001:db8:acad:1::1
PING 2001:db8:acad:1::1(2001:db8:acad:1::1) from 2001:db8:acad:1:eb1c:9d54:50c6:4cdc ens3: 56 data bytes
64 bytes from 2001:db8:acad:1::1: icmp_seq=1 ttl=64 time=12.1 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=2 ttl=64 time=1.17 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=3 ttl=64 time=1.31 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=4 ttl=64 time=1.16 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=5 ttl=64 time=1.20 ms
^C
--- 2001:db8:acad:1::1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 11ms
rtt min/avg/max/mdev = 1.155/3.394/12.140/4.373 ms
[root@localhost ~]# ping6 -I ens3 fe80::1
PING fe80::1(fe80::1) from :: ens3: 56 data bytes
64 bytes from fe80::1%ens3: icmp_seq=1 ttl=64 time=2.85 ms
64 bytes from fe80::1%ens3: icmp_seq=2 ttl=64 time=1.48 ms
64 bytes from fe80::1%ens3: icmp_seq=3 ttl=64 time=1.34 ms
64 bytes from fe80::1%ens3: icmp_seq=4 ttl=64 time=1.23 ms
64 bytes from fe80::1%ens3: icmp_seq=5 ttl=64 time=1.37 ms
^C
--- fe80::1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 11ms
rtt min/avg/max/mdev = 1.231/1.653/2.848/0.604 ms
```

Как видим интерфейс e0/1 маршрутизатора R1, являющий шлюзом для VM_CentOS, доступен как по GUA так и по LLA адресам.

###  Настройка и проверка Stateless DHCPv6 сервера на маршрутизаторе R1

Настройка пула и интерфейса e0/1 на маршрутизаторе R1:

```
ipv6 dhcp pool R1-STATELESS
 dns-server 2001:DB8:ACAD::254
 domain-name stateless.com
interface Ethernet0/1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 ipv6 enable
 ipv6 nd other-config-flag
 ipv6 dhcp server R1-STATELESS
```

Проверка назначения IPv6 адреса, DNS на интерфейсе ens3 VM_CentOS:

```
[root@localhost ~]# nmcli d show ens3
GENERAL.DEVICE:                         ens3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         00:50:00:00:06:00
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     ens3
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/6
WIRED-PROPERTIES.CARRIER:               on
IP6.ADDRESS[1]:                         2001:db8:acad:1:eb1c:9d54:50c6:4cdc/64
IP6.ADDRESS[2]:                         fe80::7d37:9eb2:fcf0:a45/64
IP6.GATEWAY:                            fe80::1
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 102
IP6.ROUTE[2]:                           dst = 2001:db8:acad:1::/64, nh = ::, mt = 102
IP6.ROUTE[3]:                           dst = ::/0, nh = fe80::1, mt = 102
IP6.ROUTE[4]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
IP6.DNS[1]:                             2001:db8:acad::254
```
Из листинга видно, что хост получил нужный адрес DNS-сервера.


### Настройка и проверка Stateful DHCPv6 сервера на маршрутизаторе R1.

Настройка пула и интерфейса e0/1 на маршрутизаторе R1:

```
ipv6 dhcp pool R1-STATEFUL
 address prefix 2001:DB8:ACAD:1::/64
 dns-server 2001:DB8:ACAD:1::254
 domain-name stateful.com
interface Ethernet0/1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 ipv6 enable
 ipv6 nd managed-config-flag
 ipv6 dhcp server R1-STATEFUL
```

Вывод команды *show ipv6 dhcp binding*:

```
R1#sh ipv6 dhcp binding
Client: FE80::7D37:9EB2:FCF0:A45
  DUID: 00048F6C3A95AA6A123EBE280EFABF4CE767
  Username : unassigned
  VRF : default
  IA NA: IA ID 0xB55E67FF, T1 43200, T2 69120
    Address: 2001:DB8:ACAD:1:2C03:62CE:EAF:AC55
            preferred lifetime 86400, valid lifetime 172800
            expires at Nov 04 2023 10:31 AM (172739 seconds)
```

Проверка назначения IPv6 адреса, DNS на интерфейсе ens3 VM_CentOS:

```
[root@localhost ~]# nmcli dev show ens3
GENERAL.DEVICE:                         ens3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         00:50:00:00:06:00
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     ens3
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/7
WIRED-PROPERTIES.CARRIER:               on
IP6.ADDRESS[1]:                         2001:db8:acad:1:eb1c:9d54:50c6:4cdc/64
IP6.ADDRESS[2]:                         2001:db8:acad:1:2c03:62ce:eaf:ac55/128
IP6.ADDRESS[3]:                         fe80::7d37:9eb2:fcf0:a45/64
IP6.GATEWAY:                            fe80::1
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 102
IP6.ROUTE[2]:                           dst = 2001:db8:acad:1::/64, nh = ::, mt = 102
IP6.ROUTE[3]:                           dst = ::/0, nh = fe80::1, mt = 102
IP6.ROUTE[4]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
IP6.ROUTE[5]:                           dst = 2001:db8:acad:1:2c03:62ce:eaf:ac55/128, nh = ::, mt = 102
IP6.DNS[1]:                             2001:db8:acad:1::254
```

### Настройка и проверка DHCPv6 Relay на маршрутизаторе R2

На маршрутизаторе R1 необходимо настроить пул R2-STATEFUL и интерфейс e0/0:

```
ipv6 dhcp pool R2-STATEFUL
 address prefix 2001:DB8:ACAD:3::/64
 dns-server 2001:DB8:ACAD:1::254
 domain-name stateful.com
interface Ethernet0/0
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:2::1/64
 ipv6 enable
 ipv6 dhcp server R2-STATEFUL
```

Как видно из схемы топологии сети интерйес ens5 VM_CentOS подключен через коммутатор S2 к порту e0/1 
маршрутизатора R2. Для того чтобы данный интерфейс получил настройки IPv6 по DHCPv6 необходимо на порту e0/1 R2
настроить DHCP Relay:

```
interface Ethernet0/1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
 ipv6 enable
 ipv6 nd managed-config-flag
 ipv6 dhcp relay destination 2001:DB8:ACAD:2::1 Ethernet0/0
```

Вывод команды *show ipv6 dhcp binding* на маршрутизаторе R1:

```
R1#show ipv6 dhcp binding
Client: FE80::7D37:9EB2:FCF0:A45
  DUID: 00048F6C3A95AA6A123EBE280EFABF4CE767
  Username : unassigned
  VRF : default
  IA NA: IA ID 0xB55E67FF, T1 43200, T2 69120
    Address: 2001:DB8:ACAD:1:2C03:62CE:EAF:AC55
            preferred lifetime 86400, valid lifetime 172800
            expires at Nov 04 2023 12:02 PM (169980 seconds)
  IA NA: IA ID 0xED10BDB8, T1 43200, T2 69120
    Address: 2001:DB8:ACAD:1:B0E8:7769:7139:4015
            preferred lifetime 86400, valid lifetime 172800
            expires at Nov 04 2023 11:40 AM (168653 seconds)
Client: FE80::777C:5025:AA9F:2B37
  DUID: 00048F6C3A95AA6A123EBE280EFABF4CE767
  Username : unassigned
  VRF : default
  IA NA: IA ID 0xED10BDB8, T1 43200, T2 69120
    Address: 2001:DB8:ACAD:3:AAA:DFB8:4BB:E3A4
            preferred lifetime 86400, valid lifetime 172800
            expires at Nov 04 2023 12:28 PM (171557 seconds)
```

Проверка назначения IPv6 адреса, DNS на интерфейсе ens5 VM_CentOS:

```
[root@localhost ~]# nmcli d show ens5
GENERAL.DEVICE:                         ens5
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         00:50:00:00:05:02
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     Profile 2
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/22
WIRED-PROPERTIES.CARRIER:               on
IP6.ADDRESS[1]:                         2001:db8:acad:3:eebc:b342:7188:2f5e/64
IP6.ADDRESS[2]:                         2001:db8:acad:3:aaa:dfb8:4bb:e3a4/128
IP6.ADDRESS[3]:                         fe80::777c:5025:aa9f:2b37/64
IP6.GATEWAY:                            fe80::1
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 102
IP6.ROUTE[2]:                           dst = 2001:db8:acad:3::/64, nh = ::, mt = 102
IP6.ROUTE[3]:                           dst = ::/0, nh = fe80::1, mt = 102
IP6.ROUTE[4]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
IP6.ROUTE[5]:                           dst = 2001:db8:acad:3:aaa:dfb8:4bb:e3a4/128, nh = ::, mt = 102
IP6.DNS[1]:                             2001:db8:acad::254
```

Проверка достпуности шлюза с VM_CentOS:

```
[root@localhost ~]# ping6 -I ens5 2001:db8:acad:3::1
PING 2001:db8:acad:3::1(2001:db8:acad:3::1) from 2001:db8:acad:3:aaa:dfb8:4bb:e3a4 ens5: 56 data bytes
64 bytes from 2001:db8:acad:3::1: icmp_seq=1 ttl=64 time=2.02 ms
64 bytes from 2001:db8:acad:3::1: icmp_seq=2 ttl=64 time=1.30 ms
64 bytes from 2001:db8:acad:3::1: icmp_seq=3 ttl=64 time=1.29 ms
64 bytes from 2001:db8:acad:3::1: icmp_seq=4 ttl=64 time=1.50 ms
^C
--- 2001:db8:acad:3::1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 9ms
rtt min/avg/max/mdev = 1.286/1.524/2.018/0.299 ms
```