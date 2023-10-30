# Настройка DHCPv4

----

## Задание:

* Создание сети и настройка базовых параметров устройств.
* Настройка и проверка двух серверов DHCPv4 на маршрутизаторе R1.
* Настройка и проверка DHCP Relay на маршрутизаторе R2.


### Схема реализации (EVE-NG)

![Scheme_DHCPv4.png](img%2FScheme_DHCPv4.png)

Таблица распределения адресного пространства 

| Device | Interface | IP Address   | Subnet Mask     | Default Gateway |
|--------|-----------|--------------|-----------------|-----------------|
| R1     | e0/0      | 10.0.0.1     | 255.255.255.252 | N/A             |
| -      | e0/1      | N/A          | N/A             | -               |
| -      | e0/1.100  | 192.168.1.1  | 255.255.255.192 | -               |
| -      | e0/1.200  | 192.168.1.65 | 255.255.255.224 | -               |
| -      | e0/1.1000 | N/A          | N/A             | -               |
| R2     | e0/0      | 10.0.0.2     | 255.255.255.252 | N/A             |
| -      | e0/1      | 192.168.1.97 | 255.255.255.240 | -               |
| S1     | VLAN 200  | 192.168.1.66 | 255.255.255.224 | 192.168.1.65    |
| S2     | VLAN 1    | 192.168.1.98 | 255.255.255.240 | 192.168.1.97    |
| PC-A   | NIC       | DHCP         | DHCP            | DHCP            |
| PC-B   | NIC       | DHCP         | DHCP            | DHCP            |

Таблица VLAN

| VLAN | Name        | Interface Assigned    |
|------|-------------|-----------------------|
| 1    | N/A         | S2: e0/1, VLAN 1      |
| 100  | Clients     | S1: e0/1              |
| 200  | Management  | S1: VLAN 200          |
| 999  | Parking_Lot | S1: e0/2-3, S2:e0/2-3 |
| 1000 | Native      | N/A                   |


Настройка сетевых устройств S1, S2, R1, R2, приведена по [ссылке](Configs%2Freadme.md).


### Настройка двух серверов DHCPv4 на маршрутизаторе R1

#### Настройка сервера DHCPv4 на R1 для клиентов сети VLAN 100:

```
ip dhcp excluded-address 192.168.1.1 192.168.1.5
ip dhcp pool R1_Client_LAN
 network 192.168.1.0 255.255.255.192
 domain-name ccna-lab.com
 default-router 192.168.1.1
 lease 2 12 30
```
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