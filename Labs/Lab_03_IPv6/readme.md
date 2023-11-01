# Настройка SLAAC, SLAAC+DHCPv6, Stateful DHCPv6

----

## Задание:

* Создание сети и настройка базовых параметров устройств.
* Проверка назначения адреса SLAAC с помощью R1.
* Настройка и проверка Stateless DHCPv6 сервера на маршрутизаторе R1.
* Настройка и проверка Stateful DHCPv6 сервера на маршрутизаторе R1.
* Настройка и проверка DHCPv6 Relay на маршрутизаторе R2.
 


### Схема реализации (EVE-NG)

![Scheme_DHCPv4.png](img%2FScheme_DHCPv4.png)

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

Проверка назначения IPv6 адреса на хосте Ubuntu:

```
root@ubuntu:~# ifconfig
ens3      Link encap:Ethernet  HWaddr 00:50:00:00:07:00
          inet6 addr: 2001:db8:acad:1:250:ff:fe00:700/64 Scope:Global
          inet6 addr: fe80::250:ff:fe00:700/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:784 errors:0 dropped:20 overruns:0 frame:0
          TX packets:797 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:108801 (108.8 KB)  TX bytes:90636 (90.6 KB)
```

Из листинга видно, что хост получил от R1 префикс и сгенерировал GUA с помощью алгоритма EUI-64. LLA также
сформирован посредством использования EUI-64.\
Проверка достпуности шлюза с Ubuntu:

```
root@ubuntu:~# ping6 -I ens3 2001:DB8:ACAD:1::1
PING 2001:DB8:ACAD:1::1(2001:db8:acad:1::1) from 2001:db8:acad:1:250:ff:fe00:700 ens3: 56 data bytes
64 bytes from 2001:db8:acad:1::1: icmp_seq=1 ttl=64 time=1.27 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=2 ttl=64 time=1.16 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=3 ttl=64 time=1.30 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=4 ttl=64 time=1.26 ms
64 bytes from 2001:db8:acad:1::1: icmp_seq=5 ttl=64 time=1.20 ms
^C
--- 2001:DB8:ACAD:1::1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.163/1.241/1.300/0.055 ms
root@ubuntu:~# ping6 -I ens3 fe80::1
PING fe80::1(fe80::1) from fe80::250:ff:fe00:700 ens3: 56 data bytes
64 bytes from fe80::1: icmp_seq=1 ttl=64 time=5.60 ms
64 bytes from fe80::1: icmp_seq=2 ttl=64 time=1.07 ms
64 bytes from fe80::1: icmp_seq=3 ttl=64 time=1.23 ms
64 bytes from fe80::1: icmp_seq=4 ttl=64 time=1.20 ms
64 bytes from fe80::1: icmp_seq=5 ttl=64 time=1.15 ms
^C
--- fe80::1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.070/2.055/5.609/1.778 ms
```

Как видим интерфейс e0/1 маршрутизатора R1, являющий шлюзом для Ubuntu, доступен как по GUA так и по LLA адресам.

#### Настройка сервера DHCPv4 на R1 для клиентов сети VLAN 1 маршрутизатора R2:

```
ip dhcp excluded-address 192.168.1.97 192.168.1.101
ip dhcp pool R2_Client_LAN
 network 192.168.1.96 255.255.255.240
 default-router 192.168.1.97
 domain-name ccna-lab.com
 lease 2 12 30
```


### Проверка получения IP-адреса по DHCP на VPC's

#### Проверка получения IP-адреса по DHCP на VPC_1 VLAN 100:

![Verify_VPC1.png](img%2FVerify_VPC1.png)

Так как маршрутизатор не пропускает broadcast-трафик, для получения IP-адреса на VPC_2 необходимо
настроить DHCP Relay на маршрутизаторе R2 интерфейс e0/1:

```
interface Ethernet0/1
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
```

Проверка получения IP-адреса по DHCP на VPC_2 VLAN 1:

![Verify_VPC2.png](img%2FVerify_VPC2.png)

Вывод команды *show ip dhcp binding* на R1:

```
R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.6         0100.5079.6668.05       Nov 02 2023 03:53 AM    Automatic
192.168.1.102       0100.5079.6668.06       Nov 02 2023 04:04 AM    Automatic
```
Вывод команды *show ip dhcp server statistics* на R1:

```
R1#show ip dhcp server statistics
Memory usage         42093
Address pools        2
Database agents      0
Automatic bindings   2
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         36
DHCPREQUEST          19
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            4
DHCPACK              19
DHCPNAK              0
```

Вывод команды *show ip dhcp server statistics* на R2:

```
R2#show ip dhcp server statistics
Memory usage         22565
Address pools        0
Database agents      0
Automatic bindings   0
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         0
DHCPREQUEST          0
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            0
DHCPACK              0
DHCPNAK              0

```