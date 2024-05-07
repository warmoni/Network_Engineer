# Тема. Организация сети до удалённых филиалов логистической компании 
ООО "Туда и обратно", расположенных на территории новых субъектов Российской Федерации

----

## Задачи:

* Разработка архитектуры сети проекта.
* Разработка адресного пространства IPv4..
* Настройка BGP-сессий между провайдерами.
* Настройка сетей центрального офиса и удалённых филиалов.
* Достижение IP-связности всех узлов компании с помощью DMVPN.


### Разработка архитектуры сети проекта
\
![BigNet.png](img%2FBigNet.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).


### Разработка адресного пространства IPv4


|                                               TTK AS15774                                               |                                         
|------------------------------------------------|--------------|-----------------|----------------|------|
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| TTK_R1                                         | E0/0         | To_MSK-IX       | 195.208.24.2   | /21  |
|                                                | E0/1         | To_RST_R1       | 1.5.51.2       | /30  |
|                                               Мегафон AS31133                                           |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| Megafon_R1                                     | E0/0         | To_Megafon_R2   | 85.26.242.1    | /30  |
|                                                | E0/1         | To_MSK-IX       | 195.208.24.3   | /21  |
|                                                | E0/2         | To_L-com_R1     | 85.26.242.5    | /30  |
|                                                | E0/3         | To_RST_R2       | 1.5.51.6       | /30  |
| Megafon_R2                                     | E0/0         | To_Megafon_R1   | 85.26.242.2    | /30  |
|                                                | E0/1         | To_UT-com_R1    | 85.26.242.9    | /30  |
|                                               РТК AS12389                                               |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| RTK_R1                                         | E0/0         | To_RTK_R1       | 79.133.64.1    | /30  |
|                                                | E0/1         | To_UT-com_R1    | 79.133.64.5    | /30  |
|                                                | E0/2         | To_MSK-IX       | 195.208.24.5   | /21  |
| RTK_R2                                         | E0/0         | To_RTK_R2       | 79.133.64.2    | /30  |
|                                                | E0/1         | To_T-com_R1     | 79.133.64.9    | /30  |
|                                                | E0/2         | To_Miranda_R1   | 79.133.64.13   | /30  |
|                                               Миранда AS201776                                          |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| Miranda_R1                                     | E0/0         | To_ZPR_R1       | 94.126.24.1    | /30  |
|                                                | E0/1         | To_RTK_R2       | 79.133.64.14   | /30  |
|                                               К-Телеком AS43733                                         |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| K-Telecom_R1                                   | E0/0         | To_MSK-IX       | 195.208.24.6   | /21  |
|                                                | E0/1         | To_HRS_R1       | 46.130.0.1     | /30  |
|                                               L-com AS4321                                              |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| Lugacom_R1                                     | E0/0         | To_LPR_R1       | 5.42.216.1     | /30  |
|                                                | E0/1         | To_UT-com_R1    | 176.96.184.10  | /30  |
|                                                | E0/2         | To_Megafon_R1   | 85.26.242.6    | /30  |
|                                               UT-com AS26810                                            |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| Ugletelecom_R1                                 | E0/0         | To_DPR_R1       | 176.96.184.5   | /30  |
|                                                | E0/1         | To_Megafon_R2   | 85.26.242.10   | /30  |
|                                                | E0/2         | To_T-com_R1     | 176.96.184.1   | /30  |
|                                                | E0/3         | To_RTK_R1       | 79.133.64.6    | /30  |
|                                                | E1/0         | To_L-com_R1     | 176.96.184.9   | /30  |
|                                                | E1/1         | To_MSK-IX       | 195.208.24.4   | /21  |
|                                               T-com AS22279                                             |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| Comtel_R1                                      | E0/0         | To_DPR_R1       | 31.133.48.1    | /30  |
|                                                | E0/1         | To_RTK_R2       | 79.133.64.10   | /30  |
|                                                | E0/2         | To_UT-com_R1    | 176.96.184.2   | /30  |
|                                               MSK-IX AS8985                                             |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| RS                                             | E0           | Route Server    | 195.208.24.1   | /21  |
|                                               Центральный офис в Ростовской области (AS1551)            |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| RST_R1                                         | e0/0         | To-TTK_R1       | 1.5.51.1       | /30  |
|                                                | e0/1         | To-RST_DSW1     | 172.16.1.9     | /30  |
|                                                | e0/2         | To-RST_DHCP     | 172.16.1.5     | /30  |
|                                                | e0/3         | To-RST_R2       | 172.16.1.1     | /30  |
|                                                | Lo 1         | MNG             | 172.16.1.254   | /32  |
| RST_R2                                         | e0/0         | To_Megafon_R1   | 1.5.51.5       | /30  |
|                                                | e0/1         | To-RST_DSW2     | 172.16.1.17    | /30  |
|                                                | e0/2         | To-RST_DHCP     | 172.16.1.53    | /30  |
|                                                | e0/3         | To-RST_R1       | 172.16.1.2     | /30  |
|                                                | Lo 0         | For_Tun1        | 1.5.51.254     | /32  |
|                                                | Lo 1         | MNG             | 172.16.1.253   | /32  |
|                                                | Tunnel 1     |                 | 10.200.0.1     | /27  |
| RST_DHCP                                       | e0/0         | To-RST_R1       | 172.16.1.6     | /30  |
|                                                | e0/1         | To-RST_R2       | 172.16.1.54    | /30  |
|                                                | e0/2         | To-RST_DSW1     | 172.16.1.21    | /30  |
|                                                | e0/3         | To-RST_DSW2     | 172.16.1.13    | /30  |
|                                                | Lo1          | MNG             | 172.16.1.247   | /32  |
| RST_DSW1                                       | e0/0         | To-RST_R1       | 172.16.1.10    | /30  |
|                                                | e0/1         | To-RST_DHCP     | 172.16.1.22    | /30  |
|                                                | Po1          | To-RST_DSW2     | 172.16.1.25    | /30  |
|                                                | e1/0         | To-RST_ASW1     | 172.16.1.29    | /30  |
|                                                | e1/1         | To-RST_ASW2     | 172.16.1.33    | /30  |
|                                                | e1/2         | To-RST_ASW3     | 172.16.1.37    | /30  |
|                                                | Lo1          | MNG             | 172.16.1.252   | /32  |
| RST_DSW2                                       | e0/0         | To-RST_R2       | 172.16.1.18    | /30  |
|                                                | e0/1         | To-RST_DHCP     | 172.16.1.14    | /30  |
|                                                | Po1          | To-RST_DSW1     | 172.16.1.26    | /30  |
|                                                | e1/0         | To-RST_ASW1     | 172.16.1.41    | /30  |
|                                                | e1/1         | To-RST_ASW2     | 172.16.1.45    | /30  |
|                                                | e1/2         | To-RST_ASW3     | 172.16.1.49    | /30  |
|                                                | Lo1          | MNG             | 172.16.1.251   | /32  |
| RST_ASW1                                       | e0/0         | To-RST_DSW1     | 172.16.1.30    | /30  |
|                                                | e0/1         | To-RST_DSW2     | 172.16.1.42    | /30  |
|                                                | Vlan2        | Gateway_VLAN2   | 192.168.1.1    | /27  |
|                                                | Vlan3        | Gateway_VLAN3   | 192.168.1.33   | /27  |
|                                                | Lo1          | MNG             | 172.16.1.250   | /32  |
| RST_ASW2                                       | e0/0         | To-RST_DSW1     | 172.16.1.34    | /30  |
|                                                | e0/1         | To-RST_DSW2     | 172.16.1.46    | /30  |
|                                                | Vlan2        | Gateway_VLAN4   | 192.168.1.65   | /27  |
|                                                | Vlan3        | Gateway_VLAN5   | 192.168.1.97   | /27  |
|                                                | Lo1          | MNG             | 172.16.1.249   | /32  |
| RST_ASW3                                       | e0/0         | To-RST_DSW1     | 172.16.1.38    | /30  |
|                                                | e0/1         | To-RST_DSW2     | 172.16.1.50    | /30  |
|                                                | Vlan2        | Gateway_VLAN6   | 192.168.1.129  | /27  |
|                                                | Vlan3        | Gateway_VLAN7   | 192.168.1.161  | /27  |
|                                                | Lo1          | MNG             | 172.16.1.248   | /32  |
|                                               Филиал в ЛНР                                              |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| LPR_R1                                         | e0/0         | To_L-com_R1     | 5.42.216.2     | /30  |
|                                                | e0/1         | To-LPR_DSW1     | 172.16.2.1     | /30  |
|                                                | Lo1          | MNG             | 172.16.2.254   | /32  |
|                                                | Tunnel 1     |                 | 10.200.0.3     | /27  |
| LPR_DSW1                                       | e0/0         | To-LPR_R1       | 172.16.2.2     | /30  |
|                                                | e0/1         | To-LPR_ASW1     | 172.16.2.5     | /30  |
|                                                | e0/2         | To-LPR_ASW2     | 172.16.2.9     | /30  |
|                                                | e0/3         | To-LPR_ASW3     | 172.16.2.13    | /30  |
|                                                | Lo1          | MNG             | 172.16.2.253   | /32  |
| LPR_ASW1                                       | e0/0         | To-LPR_DSW1     | 172.16.2.6     | /30  |
|                                                | e0/1         | To-LPR_ASW2     | 172.16.2.17    | /30  |
|                                                | Vlan2        | Gateway_VLAN2   | 192.168.2.1    | /27  |
|                                                | Lo1          | MNG             | 172.16.2.252   | /32  |
| LPR_ASW2                                       | e0/0         | To-LPR_DSW1     | 172.16.2.10    | /30  |
|                                                | e0/1         | To-LPR_ASW1     | 172.16.2.18    | /30  |
|                                                | e0/2         | To-LPR_ASW3     | 172.16.2.22    | /30  |
|                                                | Vlan4        | Gateway_VLAN4   | 192.168.2.65   | /27  |
|                                                | Vlan6        | Gateway_VLAN6   | 192.168.2.97   | /27  |
|                                                | Lo1          | MNG             | 172.16.2.251   | /32  |
| LPR_ASW3                                       | e0/0         | To-LPR_DSW1     | 172.16.2.14    | /30  |
|                                                | e0/1         | To-LPR_ASW2     | 172.16.2.21    | /30  |
|                                                | Vlan3        | Gateway_VLAN3   | 192.168.2.33   | /27  |
|                                                | Lo1          | MNG             | 172.16.2.250   | /32  |
|                                               Филиал в ДНР                                              |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| DPR_R1                                         | e0/0         | To_UT-com_R1    | 176.96.184.6   | /30  |
|                                                | e0/1         | To-DPR_DSW1     | 172.16.3.1     | /30  |
|                                                | e0/2         | To-DPR_DHCP     | 172.16.3.9     | /30  |
|                                                | e1/0         | To_T-com_R1     | 31.133.48.2    | /30  |
|                                                | e1/1         | To-DPR_DSW2     | 172.16.3.5     | /30  |
|                                                | Lo0          | For_NAT         | 176.96.184.13  | /30  |
|                                                | Lo1          | MNG             | 172.16.3.254   | /32  |
|                                                | Tunnel 1     |                 | 10.200.0.4     | /27  |
| DPR_DHCP                                       | e0/0         | To-DPR_DSW1     | 172.16.3.14    | /30  |
|                                                | e0/1         | To-DPR_DSW2     | 172.16.3.18    | /30  |
|                                                | e0/2         | To-DPR_R1       | 172.16.3.10    | /30  |
|                                                | Lo1          | MNG             | 172.16.3.248   | /32  |
| DPR_DSW1                                       | e0/0         | To-DPR_R1       | 172.16.3.2     | /30  |
|                                                | e0/1         | To-DPR_DHCP     | 172.16.3.13    | /30  |
|                                                | e0/2         | To-DPR_ASW1     | 172.16.3.21    | /30  |
|                                                | e0/3         | To-DPR_ASW2     | 172.16.3.25    | /30  |
|                                                | e1/0         | To-DPR_ASW3     | 172.16.3.29    | /30  |
|                                                | Lo1          | MNG             | 172.16.3.253   | /32  |
| DPR_DSW2                                       | e0/0         | TO-DPR_R1       | 172.16.3.6     | /30  |
|                                                | e0/1         | To-DPR_DHCP     | 172.16.3.17    | /30  |
|                                                | e0/2         | To-DPR_ASW1     | 172.16.3.33    | /30  |
|                                                | e0/3         | To-DPR_ASW2     | 172.16.3.37    | /30  |
|                                                | e1/0         | To-DPR_ASW3     | 172.16.3.41    | /30  |
|                                                | Lo1          | MNG             | 172.16.3.252   | /32  |
| DPR_ASW1                                       | e0/0         | To-DPR_DSW1     | 172.16.3.22    | /30  |
|                                                | e1/0         | To-DPR_DSW2     | 172.16.3.34    | /30  |
|                                                | Vlan2        | Gateway_VLAN2   | 192.168.3.1    | /27  |
|                                                | Vlan3        | Gateway_VLAN3   | 192.168.3.33   | /27  |
|                                                | Lo1          | MNG             | 172.16.3.251   | /32  |
| DPR_ASW2                                       | e0/0         | To-DPR_DSW1     | 172.16.3.26    | /30  |
|                                                | e1/0         | To-DPR_DSW2     | 172.16.3.38    | /30  |
|                                                | Vlan4        | Gateway_VLAN4   | 192.168.3.65   | /27  |
|                                                | Lo1          | MNG             | 172.16.3.250   | /32  |
| DPR_ASW3                                       | e0/0         | To-DPR_DSW1     | 172.16.3.30    | /30  |
|                                                | e1/0         | To-DPR_DSW2     | 172.16.3.42    | /30  |
|                                                | Vlan6        | Gateway_VLAN6   | 172.16.3.249   | /27  |
|                                                | Lo1          | MNG             | 192.168.3.97   | /32  |
|                                               Филиал в Запорожской области                              |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| ZPR_R1                                         | e0/0         | To_Miranda_R1   | 94.126.24.2    | /30  |
|                                                | e0/1.4       | Gateway_VLAN4   | 192.168.4.65   | /27  |
|                                                | e0/1.6       | Gateway_VLAN6   | 192.168.4.129  | /27  |
|                                                | e0/1.333     | MNG             | 172.16.4.1     | /29  |
|                                                | Tunnel 1     |                 | 10.200.0.2     | /27  |
| ZPR_DSW1                                       | Vlan333      | MNG             | 172.16.4.2     | /29  |
| ZPR_ASW1                                       | Vlan333      | MNG             | 172.16.4.3     | /29  |
| ZPR_ASW2                                       | Vlan333      | MNG             | 172.16.4.4     | /29  |
|                                               Филиал в Херсонской области                               |
| Hostname                                       | L3 interface | Description     | IPv4-address   | Mask |
| ZPR_R1                                         | e0/0         | To_K-Telecom_R1 | 46.130.0.2     | /30  |
|                                                | e0/1.4       | Gateway_VLAN4   | 192.168.5.65   | /27  |
|                                                | e0/1.6       | Gateway_VLAN6   | 192.168.5.129  | /27  |
|                                                | e0/1.333     | MNG             | 172.16.5.1     | /29  |
|                                                | Tunnel 1     |                 | 10.200.0.5     | /27  |
| ZPR_DSW1                                       | Vlan333      | MNG             | 172.16.5.2     | /29  |
| ZPR_ASW1                                       | Vlan333      | MNG             | 172.16.5.3     | /29  |
| ZPR_ASW2                                       | Vlan333      | MNG             | 172.16.5.4     | /29  |










Для настройки GRE-туннелей взята сеть 10.100.0.0/30, на R15 и R18 подняты интерфейсы
Loopback 0 с белыми IP-адресами (на их основе подняты туннели). Чтоб организовать связность
внутренних сетей офисов, интерфейсы туннелей (Tunnel 1) добавлены в процесс EIGRP, 
на R15 включена редистрибьюция из OSPF в EIGRP.

R15:
```
interface Loopback0
 ip address 100.1.0.10 255.255.255.255
!
interface Tunnel1
 ip address 10.100.0.1 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback0
 tunnel destination 20.42.0.17
!
router eigrp AS2042
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Tunnel1
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  topology base
   redistribute ospf 110 metric 100000 1000 255 1 1500
  exit-af-topology
  network 10.100.0.1 0.0.0.0
  eigrp router-id 15.15.15.15
 exit-address-family
!
```


R18:
```
interface Loopback0
 ip address 20.42.0.17 255.255.255.255
!
interface Tunnel1
 ip address 10.100.0.2 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback0
 tunnel destination 100.1.0.10
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
  af-interface Tunnel1
   bandwidth-percent 20
   no passive-interface
  exit-af-interface
  !
  topology base
   redistribute static metric 100000 1000 255 1 1500
  exit-af-topology
  network 10.100.0.2 0.0.0.0
  network 172.16.3.1 0.0.0.0
  network 172.16.3.5 0.0.0.0
  network 192.168.254.18 0.0.0.0
  eigrp router-id 18.18.18.18
 exit-address-family
```

Таблица маршрутизации (EIGRP) на R15:
```
R15#sh ip route eigrp

Gateway of last resort is 100.1.0.6 to network 0.0.0.0

      10.0.0.0/8 is variably subnetted, 8 subnets, 4 masks
D        10.0.10.0/24 [90/77829120] via 10.100.0.2, 10:31:30, Tunnel1
      172.16.0.0/16 is variably subnetted, 20 subnets, 3 masks
D        172.16.3.0/24 [90/77824000] via 10.100.0.2, 10:31:30, Tunnel1
D        172.16.3.0/30 [90/77312000] via 10.100.0.2, 10:31:30, Tunnel1
D        172.16.3.4/30 [90/77312000] via 10.100.0.2, 10:31:30, Tunnel1
      192.168.254.0/32 is subnetted, 14 subnets
D        192.168.254.9 [90/77824640] via 10.100.0.2, 10:31:30, Tunnel1
D        192.168.254.10 [90/77824640] via 10.100.0.2, 10:31:30, Tunnel1
D        192.168.254.16 [90/77312640] via 10.100.0.2, 10:31:30, Tunnel1
D        192.168.254.17 [90/77312640] via 10.100.0.2, 10:31:30, Tunnel1
D        192.168.254.18 [90/76800640] via 10.100.0.2, 10:31:30, Tunnel1
D        192.168.254.32 [90/77824640] via 10.100.0.2, 10:31:30, Tunnel1
```

Таблица маршрутизации на R18:
```
R18#sh ip route eigrp

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

      10.0.0.0/8 is variably subnetted, 7 subnets, 4 masks
D EX     10.0.2.0/24 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     10.0.3.0/24 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D        10.0.10.0/24 [90/1541120] via 172.16.3.6, 15:02:12, Ethernet0/1
                      [90/1541120] via 172.16.3.2, 15:02:12, Ethernet0/0
D EX     10.0.29.0/24 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     10.200.0.0/27 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
      172.16.0.0/16 is variably subnetted, 18 subnets, 3 masks
D EX     172.16.1.0/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.4/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.8/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.12/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.16/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.20/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.24/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.28/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.32/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.36/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.40/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.44/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D EX     172.16.1.48/30 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
D        172.16.3.0/24 [90/1536000] via 172.16.3.6, 15:04:08, Ethernet0/1
                       [90/1536000] via 172.16.3.2, 15:04:08, Ethernet0/0
      192.168.3.0/29 is subnetted, 1 subnets
D EX     192.168.3.0 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
      192.168.4.0/29 is subnetted, 1 subnets
D EX     192.168.4.24 [170/81920000] via 10.100.0.1, 10:31:32, Tunnel1
      192.168.254.0/32 is subnetted, 20 subnets
D        192.168.254.9 [90/1536640] via 172.16.3.6, 15:02:23, Ethernet0/1
                       [90/1536640] via 172.16.3.2, 15:02:23, Ethernet0/0
D        192.168.254.10 [90/1536640] via 172.16.3.6, 15:02:20, Ethernet0/1
                        [90/1536640] via 172.16.3.2, 15:02:20, Ethernet0/0
D EX     192.168.254.12 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.13 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.14 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.15 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D        192.168.254.16 [90/1024640] via 172.16.3.2, 15:02:23, Ethernet0/0
D        192.168.254.17 [90/1024640] via 172.16.3.6, 15:02:23, Ethernet0/1
D EX     192.168.254.19 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.20 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.27 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D EX     192.168.254.28 [170/81920000] via 10.100.0.1, 10:31:31, Tunnel1
D        192.168.254.32 [90/1536640] via 172.16.3.2, 15:02:23, Ethernet0/0
```

Проверка IP-связности между хостами VPC9 (С.-Петербург) и VPC7 (Москва):
```
VPC7> ip dhcp
DDORA IP 10.0.2.4/24 GW 10.0.2.1

VPC7> ping 10.0.9.2

84 bytes from 10.0.9.2 icmp_seq=1 ttl=58 time=12.650 ms
84 bytes from 10.0.9.2 icmp_seq=2 ttl=58 time=15.388 ms
84 bytes from 10.0.9.2 icmp_seq=3 ttl=58 time=12.528 ms
84 bytes from 10.0.9.2 icmp_seq=4 ttl=58 time=12.909 ms
84 bytes from 10.0.9.2 icmp_seq=5 ttl=58 time=14.135 ms

VPC9> ip dhcp
DDORA IP 10.0.9.2/24 GW 10.0.9.1

VPC9> ping 10.0.2.4

84 bytes from 10.0.2.4 icmp_seq=1 ttl=58 time=10.463 ms
84 bytes from 10.0.2.4 icmp_seq=2 ttl=58 time=10.557 ms
84 bytes from 10.0.2.4 icmp_seq=3 ttl=58 time=11.366 ms
84 bytes from 10.0.2.4 icmp_seq=4 ttl=58 time=16.662 ms
84 bytes from 10.0.2.4 icmp_seq=5 ttl=58 time=9.785 ms
```


### Настройка DMVPN между офисами Москва и Чокурдах, Лабытнанги

Для организации DMVPN-туннеля (фаза 2) между офисами Москва и Чокурдах, Лабытнанги взята сеть
10.200.0.0/27, использованы интерфейсы Loopback 0 (Москва и Чокурдах) и e0/0 (Лабытнанги).
Чтобы Loopback 0 в офисе Чокурдах был доступен с Москвы, использован статический NAT.
Для организации IP-связности офисов интерфейсы туннелей (Tunnel 2) добавлены в процесс
OSPF (area 2).

R15:
```
interface Tunnel2
 ip address 10.200.0.1 255.255.255.224
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast dynamic
 ip nhrp network-id 200
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf priority 254
 ip ospf 110 area 2
 tunnel source Loopback0
 tunnel mode gre multipoint
```

R27:
```
interface Tunnel2
 ip address 10.200.0.2 255.255.255.224
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 192.168.254.15
 ip nhrp map 10.200.0.1 100.1.0.10
 ip nhrp map multicast 100.1.0.10
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf priority 0
 ip ospf 110 area 2
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 description To_R25_AS520
 ip address 52.0.0.2 255.255.255.252
 ipv6 address FE80::52:0:0:2 link-local
 ipv6 address 2001:DB8:520:52::2/112
 ipv6 enable
!
router ospf 110
 router-id 27.27.27.27
```

R28:
```
interface Loopback1
 description Lo1
 ip address 192.168.254.28 255.255.255.255
 ip ospf 110 area 2
 ipv6 address FE80::192:168:254:28 link-local
 ipv6 address FD00:3CD5:1001:0:192:168:254:28/128
 ipv6 enable
!
interface Tunnel2
 ip address 10.200.0.3 255.255.255.224
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 192.168.254.15
 ip nhrp map 10.200.0.1 100.1.0.10
 ip nhrp map multicast 100.1.0.10
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf 110 area 2
 tunnel source Loopback1
 tunnel mode gre multipoint
!
interface Ethernet0/2.29
 description Hosts_VLAN_29
 encapsulation dot1Q 29
 ip address 10.0.29.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip ospf 110 area 2
 ipv6 address FE80::10:0:29:1 link-local
 ipv6 address 2001:DB8:1001:10:0:29:0:1/96
 ipv6 enable
!
interface Ethernet0/2.333
 description Management_SW29
 encapsulation dot1Q 333
 ip address 192.168.4.28 255.255.255.248
 ip ospf 110 area 2
 ipv6 address FE80::192:168:4:28 link-local
 ipv6 address FD00:3CD5:1001:0:192:168:4:28/112
 ipv6 enable
!
router ospf 110
 router-id 28.28.28.28
!
ip nat inside source static 192.168.254.28 52.0.0.30
```

Таблица LSDB на R15:
```
R15#sh ip ospf database

            OSPF Router with ID (15.15.15.15) (Process ID 110)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
12.12.12.12     12.12.12.12     458         0x8000001F 0x00EF20 7
13.13.13.13     13.13.13.13     732         0x8000001F 0x009376 7
14.14.14.14     14.14.14.14     477         0x8000001F 0x0032C3 7
15.15.15.15     15.15.15.15     438         0x8000003D 0x006F92 7

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
10.0.2.0        12.12.12.12     458         0x8000001D 0x00BD1C
10.0.2.0        13.13.13.13     732         0x8000001D 0x009F36
10.0.3.0        12.12.12.12     458         0x8000001D 0x00B226
10.0.3.0        13.13.13.13     732         0x8000001D 0x009440
10.0.29.0       15.15.15.15     1703        0x80000015 0x00735C
10.200.0.0      15.15.15.15     1962        0x80000015 0x002B22
172.16.1.8      15.15.15.15     438         0x8000001D 0x00FE19
172.16.1.20     14.14.14.14     477         0x8000001D 0x00A46B
172.16.1.24     12.12.12.12     458         0x8000001D 0x00C250
172.16.1.24     13.13.13.13     732         0x8000001D 0x009A75
172.16.1.28     12.12.12.12     458         0x8000001D 0x009A74
172.16.1.28     13.13.13.13     732         0x8000001D 0x007299
172.16.1.32     12.12.12.12     458         0x8000001D 0x0068A3
172.16.1.32     13.13.13.13     732         0x8000001E 0x0052B3
172.16.1.36     12.12.12.12     458         0x8000001D 0x0040C7
172.16.1.36     13.13.13.13     732         0x8000001D 0x002CD6
172.16.1.40     12.12.12.12     458         0x8000001D 0x0022E0
172.16.1.40     13.13.13.13     732         0x8000001D 0x0004FA
192.168.3.0     12.12.12.12     458         0x8000001D 0x005927
192.168.3.0     13.13.13.13     732         0x8000001D 0x003B41
192.168.4.24    15.15.15.15     1703        0x80000015 0x003D3B
192.168.254.19  14.14.14.14     477         0x8000001D 0x00B4AD
192.168.254.20  15.15.15.15     438         0x8000001D 0x008CD0
192.168.254.27  15.15.15.15     1962        0x80000015 0x002656
192.168.254.28  15.15.15.15     1703        0x80000015 0x001C5F

                Router Link States (Area 2)

Link ID         ADV Router      Age         Seq#       Checksum Link count
15.15.15.15     15.15.15.15     1962        0x800000EE 0x009E98 1
27.27.27.27     27.27.27.27     1768        0x80000123 0x00A16D 2
28.28.28.28     28.28.28.28     1410        0x80000048 0x0061A5 4

                Net Link States (Area 2)

Link ID         ADV Router      Age         Seq#       Checksum
10.200.0.1      15.15.15.15     1962        0x80000166 0x00EFD5

                Summary Net Link States (Area 2)

Link ID         ADV Router      Age         Seq#       Checksum
10.0.2.0        15.15.15.15     438         0x8000001A 0x00CDF8
10.0.3.0        15.15.15.15     438         0x8000001A 0x00C203
172.16.1.0      15.15.15.15     1703        0x80000015 0x005FC8
172.16.1.4      15.15.15.15     1703        0x80000015 0x0037EC
172.16.1.8      15.15.15.15     1703        0x80000015 0x000F11
172.16.1.12     15.15.15.15     1703        0x80000015 0x004BC6
172.16.1.16     15.15.15.15     1703        0x80000015 0x0023EA
172.16.1.20     15.15.15.15     1703        0x80000015 0x00FA0F
172.16.1.24     15.15.15.15     1703        0x80000015 0x00D233
172.16.1.28     15.15.15.15     1703        0x80000015 0x00AA57
172.16.1.32     15.15.15.15     1703        0x80000015 0x00827B
172.16.1.36     15.15.15.15     1703        0x80000015 0x005A9F
172.16.1.40     15.15.15.15     1703        0x80000015 0x003CB8
172.16.1.44     15.15.15.15     1703        0x80000015 0x00A556
172.16.1.48     15.15.15.15     1703        0x80000015 0x00E10C
192.168.3.0     15.15.15.15     1703        0x80000015 0x0073FE
192.168.254.12  15.15.15.15     1703        0x80000015 0x00EC80
192.168.254.13  15.15.15.15     1703        0x80000015 0x00E289
192.168.254.14  15.15.15.15     1703        0x80000015 0x00D892
192.168.254.15  15.15.15.15     1703        0x80000015 0x006A0A
192.168.254.19  15.15.15.15     1703        0x80000015 0x000B51
192.168.254.20  15.15.15.15     1703        0x80000015 0x009CC8

                Summary ASB Link States (Area 2)

Link ID         ADV Router      Age         Seq#       Checksum
14.14.14.14     15.15.15.15     438         0x8000001A 0x006341

                Router Link States (Area 102)

Link ID         ADV Router      Age         Seq#       Checksum Link count
15.15.15.15     15.15.15.15     438         0x8000001E 0x005E40 2
20.20.20.20     20.20.20.20     579         0x8000001E 0x0055A9 3

                Summary Net Link States (Area 102)

Link ID         ADV Router      Age         Seq#       Checksum
10.0.2.0        15.15.15.15     438         0x8000001D 0x00C7FB
10.0.3.0        15.15.15.15     438         0x8000001D 0x00BC06
10.0.29.0       15.15.15.15     1703        0x80000015 0x00735C
10.200.0.0      15.15.15.15     1962        0x80000015 0x002B22
172.16.1.0      15.15.15.15     438         0x8000001D 0x004FD0
172.16.1.4      15.15.15.15     438         0x8000001D 0x0027F4
172.16.1.12     15.15.15.15     438         0x8000001D 0x003BCE
172.16.1.16     15.15.15.15     438         0x8000001D 0x0013F2
172.16.1.24     15.15.15.15     438         0x8000001D 0x00C23B
172.16.1.28     15.15.15.15     438         0x8000001D 0x009A5F
172.16.1.32     15.15.15.15     438         0x8000001D 0x007283
172.16.1.36     15.15.15.15     438         0x8000001D 0x004AA7
172.16.1.40     15.15.15.15     438         0x8000001D 0x002CC0
172.16.1.44     15.15.15.15     438         0x8000001D 0x00955E
172.16.1.48     15.15.15.15     438         0x8000001D 0x00D114
192.168.3.0     15.15.15.15     438         0x8000001D 0x006307
192.168.4.24    15.15.15.15     1703        0x80000015 0x003D3B
192.168.254.12  15.15.15.15     438         0x8000001D 0x00DC88
192.168.254.13  15.15.15.15     438         0x8000001D 0x00D291
192.168.254.14  15.15.15.15     438         0x8000001D 0x00C89A
192.168.254.15  15.15.15.15     438         0x8000001D 0x005A12
192.168.254.27  15.15.15.15     1962        0x80000015 0x002656
192.168.254.28  15.15.15.15     1703        0x80000015 0x001C5F

                Summary ASB Link States (Area 102)

Link ID         ADV Router      Age         Seq#       Checksum
14.14.14.14     15.15.15.15     438         0x8000001D 0x005D44

                Type-5 AS External Link States

Link ID         ADV Router      Age         Seq#       Checksum Tag
0.0.0.0         15.15.15.15     438         0x8000001D 0x00F0FB 110
```


Проверка состояния DMVPN-туннелей на R15:
```
R15#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel2, IPv4 NHRP Details
Type:Hub, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 52.0.0.2             10.200.0.2    UP 11:55:18     D
     1 52.0.0.30            10.200.0.3    UP 11:55:24    DN
```

Проверка IP-связности сетей офисов с хоста VPC1 (Москва) к R27 (Лабытнанги) и VPC30 (Чокурдах):
```
VPC1> ip dhcp
DORA IP 10.0.3.129/24 GW 10.0.3.1

VPC1> ping 192.168.254.27

84 bytes from 192.168.254.27 icmp_seq=1 ttl=252 time=13.448 ms
84 bytes from 192.168.254.27 icmp_seq=2 ttl=252 time=13.645 ms
84 bytes from 192.168.254.27 icmp_seq=3 ttl=252 time=8.870 ms
84 bytes from 192.168.254.27 icmp_seq=4 ttl=252 time=14.005 ms
84 bytes from 192.168.254.27 icmp_seq=5 ttl=252 time=83.848 ms

VPC1> ping 10.0.29.3

84 bytes from 10.0.29.3 icmp_seq=1 ttl=60 time=14.570 ms
84 bytes from 10.0.29.3 icmp_seq=2 ttl=60 time=13.357 ms
84 bytes from 10.0.29.3 icmp_seq=3 ttl=60 time=11.718 ms
84 bytes from 10.0.29.3 icmp_seq=4 ttl=60 time=15.079 ms
84 bytes from 10.0.29.3 icmp_seq=5 ttl=60 time=12.306 ms

VPC1> ping 10.0.29.2

84 bytes from 10.0.29.2 icmp_seq=1 ttl=60 time=16.161 ms
84 bytes from 10.0.29.2 icmp_seq=2 ttl=60 time=10.476 ms
84 bytes from 10.0.29.2 icmp_seq=3 ttl=60 time=9.529 ms
84 bytes from 10.0.29.2 icmp_seq=4 ttl=60 time=18.292 ms
84 bytes from 10.0.29.2 icmp_seq=5 ttl=60 time=9.194 ms

```