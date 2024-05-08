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


![Project.png](img%2FProject.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).


### Разработка адресного пространства IPv4


|                                                  |                |                   TTK AS15774                   |                  |        |    
|:------------------------------------------------:|:--------------:|:-----------------------------------------------:|:----------------:|:------:|
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      TTK_R1                      |      E0/0      |                    To_MSK-IX                    |   195.208.24.2   |  /21   |
|                                                  |      E0/1      |                    To_RST_R1                    |     1.5.51.2     |  /30   |
|                                                  |                |                 Мегафон AS31133                 |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                    Megafon_R1                    |      E0/0      |                  To_Megafon_R2                  |   85.26.242.1    |  /30   |
|                                                  |      E0/1      |                    To_MSK-IX                    |   195.208.24.3   |  /21   |
|                                                  |      E0/2      |                   To_L-com_R1                   |   85.26.242.5    |  /30   |
|                                                  |      E0/3      |                    To_RST_R2                    |     1.5.51.6     |  /30   |
|                    Megafon_R2                    |      E0/0      |                  To_Megafon_R1                  |   85.26.242.2    |  /30   |
|                                                  |      E0/1      |                  To_UT-com_R1                   |   85.26.242.9    |  /30   |
|                                                  |                |                   РТК AS12389                   |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      RTK_R1                      |      E0/0      |                    To_RTK_R1                    |   79.133.64.1    |  /30   |
|                                                  |      E0/1      |                  To_UT-com_R1                   |   79.133.64.5    |  /30   |
|                                                  |      E0/2      |                    To_MSK-IX                    |   195.208.24.5   |  /21   |
|                      RTK_R2                      |      E0/0      |                    To_RTK_R2                    |   79.133.64.2    |  /30   |
|                                                  |      E0/1      |                   To_T-com_R1                   |   79.133.64.9    |  /30   |
|                                                  |      E0/2      |                  To_Miranda_R1                  |   79.133.64.13   |  /30   |
|                                                  |                |                Миранда AS201776                 |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                    Miranda_R1                    |      E0/0      |                    To_ZPR_R1                    |   94.126.24.1    |  /30   |
|                                                  |      E0/1      |                    To_RTK_R2                    |   79.133.64.14   |  /30   |
|                                                  |                |                К-Телеком AS43733                |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                   K-Telecom_R1                   |      E0/0      |                    To_MSK-IX                    |   195.208.24.6   |  /21   |
|                                                  |      E0/1      |                    To_HRS_R1                    |    46.130.0.1    |  /30   |
|                                                  |                |                  L-com AS4321                   |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                    Lugacom_R1                    |      E0/0      |                    To_LPR_R1                    |    5.42.216.1    |  /30   |
|                                                  |      E0/1      |                  To_UT-com_R1                   |  176.96.184.10   |  /30   |
|                                                  |      E0/2      |                  To_Megafon_R1                  |   85.26.242.6    |  /30   |
|                                                  |                |                 UT-com AS26810                  |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                  Ugletelecom_R1                  |      E0/0      |                    To_DPR_R1                    |   176.96.184.5   |  /30   |
|                                                  |      E0/1      |                  To_Megafon_R2                  |   85.26.242.10   |  /30   |
|                                                  |      E0/2      |                   To_T-com_R1                   |   176.96.184.1   |  /30   |
|                                                  |      E0/3      |                    To_RTK_R1                    |   79.133.64.6    |  /30   |
|                                                  |      E1/0      |                   To_L-com_R1                   |   176.96.184.9   |  /30   |
|                                                  |      E1/1      |                    To_MSK-IX                    |   195.208.24.4   |  /21   |
|                                                  |                |                  T-com AS22279                  |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                    Comtel_R1                     |      E0/0      |                    To_DPR_R1                    |   31.133.48.1    |  /30   |
|                                                  |      E0/1      |                    To_RTK_R2                    |   79.133.64.10   |  /30   |
|                                                  |      E0/2      |                  To_UT-com_R1                   |   176.96.184.2   |  /30   |
|                                                  |                |                  MSK-IX AS8985                  |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                        RS                        |       E0       |                  Route Server                   |   195.208.24.1   |  /21   |
|                                                  |                | Центральный офис в Ростовской области (AS1551)  |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      RST_R1                      |      e0/0      |                    To-TTK_R1                    |     1.5.51.1     |  /30   |
|                                                  |      e0/1      |                   To-RST_DSW1                   |    172.16.1.9    |  /30   |
|                                                  |      e0/2      |                   To-RST_DHCP                   |    172.16.1.5    |  /30   |
|                                                  |      e0/3      |                    To-RST_R2                    |    172.16.1.1    |  /30   |
|                                                  |      Lo 1      |                       MNG                       |   172.16.1.254   |  /32   |
|                      RST_R2                      |      e0/0      |                  To_Megafon_R1                  |     1.5.51.5     |  /30   |
|                                                  |      e0/1      |                   To-RST_DSW2                   |   172.16.1.17    |  /30   |
|                                                  |      e0/2      |                   To-RST_DHCP                   |   172.16.1.53    |  /30   |
|                                                  |      e0/3      |                    To-RST_R1                    |    172.16.1.2    |  /30   |
|                                                  |      Lo 0      |                    For_Tun1                     |    1.5.51.254    |  /32   |
|                                                  |      Lo 1      |                       MNG                       |   172.16.1.253   |  /32   |
|                                                  |    Tunnel 1    |                                                 |    10.200.0.1    |  /27   |
|                     RST_DHCP                     |      e0/0      |                    To-RST_R1                    |    172.16.1.6    |  /30   |
|                                                  |      e0/1      |                    To-RST_R2                    |   172.16.1.54    |  /30   |
|                                                  |      e0/2      |                   To-RST_DSW1                   |   172.16.1.21    |  /30   |
|                                                  |      e0/3      |                   To-RST_DSW2                   |   172.16.1.13    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.247   |  /32   |
|                     RST_DSW1                     |      e0/0      |                    To-RST_R1                    |   172.16.1.10    |  /30   |
|                                                  |      e0/1      |                   To-RST_DHCP                   |   172.16.1.22    |  /30   |
|                                                  |      Po1       |                   To-RST_DSW2                   |   172.16.1.25    |  /30   |
|                                                  |      e1/0      |                   To-RST_ASW1                   |   172.16.1.29    |  /30   |
|                                                  |      e1/1      |                   To-RST_ASW2                   |   172.16.1.33    |  /30   |
|                                                  |      e1/2      |                   To-RST_ASW3                   |   172.16.1.37    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.252   |  /32   |
|                     RST_DSW2                     |      e0/0      |                    To-RST_R2                    |   172.16.1.18    |  /30   |
|                                                  |      e0/1      |                   To-RST_DHCP                   |   172.16.1.14    |  /30   |
|                                                  |      Po1       |                   To-RST_DSW1                   |   172.16.1.26    |  /30   |
|                                                  |      e1/0      |                   To-RST_ASW1                   |   172.16.1.41    |  /30   |
|                                                  |      e1/1      |                   To-RST_ASW2                   |   172.16.1.45    |  /30   |
|                                                  |      e1/2      |                   To-RST_ASW3                   |   172.16.1.49    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.251   |  /32   |
|                     RST_ASW1                     |      e0/0      |                   To-RST_DSW1                   |   172.16.1.30    |  /30   |
|                                                  |      e0/1      |                   To-RST_DSW2                   |   172.16.1.42    |  /30   |
|                                                  |     Vlan2      |                  Gateway_VLAN2                  |   192.168.1.1    |  /27   |
|                                                  |     Vlan3      |                  Gateway_VLAN3                  |   192.168.1.33   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.250   |  /32   |
|                     RST_ASW2                     |      e0/0      |                   To-RST_DSW1                   |   172.16.1.34    |  /30   |
|                                                  |      e0/1      |                   To-RST_DSW2                   |   172.16.1.46    |  /30   |
|                                                  |     Vlan2      |                  Gateway_VLAN4                  |   192.168.1.65   |  /27   |
|                                                  |     Vlan3      |                  Gateway_VLAN5                  |   192.168.1.97   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.249   |  /32   |
|                     RST_ASW3                     |      e0/0      |                   To-RST_DSW1                   |   172.16.1.38    |  /30   |
|                                                  |      e0/1      |                   To-RST_DSW2                   |   172.16.1.50    |  /30   |
|                                                  |     Vlan2      |                  Gateway_VLAN6                  |  192.168.1.129   |  /27   |
|                                                  |     Vlan3      |                  Gateway_VLAN7                  |  192.168.1.161   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.1.248   |  /32   |
|                                                  |                |                  Филиал в ЛНР                   |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      LPR_R1                      |      e0/0      |                   To_L-com_R1                   |    5.42.216.2    |  /30   |
|                                                  |      e0/1      |                   To-LPR_DSW1                   |    172.16.2.1    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.2.254   |  /32   |
|                                                  |    Tunnel 1    |                                                 |    10.200.0.3    |  /27   |
|                     LPR_DSW1                     |      e0/0      |                    To-LPR_R1                    |    172.16.2.2    |  /30   |
|                                                  |      e0/1      |                   To-LPR_ASW1                   |    172.16.2.5    |  /30   |
|                                                  |      e0/2      |                   To-LPR_ASW2                   |    172.16.2.9    |  /30   |
|                                                  |      e0/3      |                   To-LPR_ASW3                   |   172.16.2.13    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.2.253   |  /32   |
|                     LPR_ASW1                     |      e0/0      |                   To-LPR_DSW1                   |    172.16.2.6    |  /30   |
|                                                  |      e0/1      |                   To-LPR_ASW2                   |   172.16.2.17    |  /30   |
|                                                  |     Vlan2      |                  Gateway_VLAN2                  |   192.168.2.1    |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.2.252   |  /32   |
|                     LPR_ASW2                     |      e0/0      |                   To-LPR_DSW1                   |   172.16.2.10    |  /30   |
|                                                  |      e0/1      |                   To-LPR_ASW1                   |   172.16.2.18    |  /30   |
|                                                  |      e0/2      |                   To-LPR_ASW3                   |   172.16.2.22    |  /30   |
|                                                  |     Vlan4      |                  Gateway_VLAN4                  |   192.168.2.65   |  /27   |
|                                                  |     Vlan6      |                  Gateway_VLAN6                  |   192.168.2.97   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.2.251   |  /32   |
|                     LPR_ASW3                     |      e0/0      |                   To-LPR_DSW1                   |   172.16.2.14    |  /30   |
|                                                  |      e0/1      |                   To-LPR_ASW2                   |   172.16.2.21    |  /30   |
|                                                  |     Vlan3      |                  Gateway_VLAN3                  |   192.168.2.33   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.2.250   |  /32   |
|                                                  |                |                  Филиал в ДНР                   |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      DPR_R1                      |      e0/0      |                  To_UT-com_R1                   |   176.96.184.6   |  /30   |
|                                                  |      e0/1      |                   To-DPR_DSW1                   |    172.16.3.1    |  /30   |
|                                                  |      e0/2      |                   To-DPR_DHCP                   |    172.16.3.9    |  /30   |
|                                                  |      e1/0      |                   To_T-com_R1                   |   31.133.48.2    |  /30   |
|                                                  |      e1/1      |                   To-DPR_DSW2                   |    172.16.3.5    |  /30   |
|                                                  |      Lo0       |                     For_NAT                     |  176.96.184.13   |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.254   |  /32   |
|                                                  |    Tunnel 1    |                                                 |    10.200.0.4    |  /27   |
|                     DPR_DHCP                     |      e0/0      |                   To-DPR_DSW1                   |   172.16.3.14    |  /30   |
|                                                  |      e0/1      |                   To-DPR_DSW2                   |   172.16.3.18    |  /30   |
|                                                  |      e0/2      |                    To-DPR_R1                    |   172.16.3.10    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.248   |  /32   |
|                     DPR_DSW1                     |      e0/0      |                    To-DPR_R1                    |    172.16.3.2    |  /30   |
|                                                  |      e0/1      |                   To-DPR_DHCP                   |   172.16.3.13    |  /30   |
|                                                  |      e0/2      |                   To-DPR_ASW1                   |   172.16.3.21    |  /30   |
|                                                  |      e0/3      |                   To-DPR_ASW2                   |   172.16.3.25    |  /30   |
|                                                  |      e1/0      |                   To-DPR_ASW3                   |   172.16.3.29    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.253   |  /32   |
|                     DPR_DSW2                     |      e0/0      |                    TO-DPR_R1                    |    172.16.3.6    |  /30   |
|                                                  |      e0/1      |                   To-DPR_DHCP                   |   172.16.3.17    |  /30   |
|                                                  |      e0/2      |                   To-DPR_ASW1                   |   172.16.3.33    |  /30   |
|                                                  |      e0/3      |                   To-DPR_ASW2                   |   172.16.3.37    |  /30   |
|                                                  |      e1/0      |                   To-DPR_ASW3                   |   172.16.3.41    |  /30   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.252   |  /32   |
|                     DPR_ASW1                     |      e0/0      |                   To-DPR_DSW1                   |   172.16.3.22    |  /30   |
|                                                  |      e1/0      |                   To-DPR_DSW2                   |   172.16.3.34    |  /30   |
|                                                  |     Vlan2      |                  Gateway_VLAN2                  |   192.168.3.1    |  /27   |
|                                                  |     Vlan3      |                  Gateway_VLAN3                  |   192.168.3.33   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.251   |  /32   |
|                     DPR_ASW2                     |      e0/0      |                   To-DPR_DSW1                   |   172.16.3.26    |  /30   |
|                                                  |      e1/0      |                   To-DPR_DSW2                   |   172.16.3.38    |  /30   |
|                                                  |     Vlan4      |                  Gateway_VLAN4                  |   192.168.3.65   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   172.16.3.250   |  /32   |
|                     DPR_ASW3                     |      e0/0      |                   To-DPR_DSW1                   |   172.16.3.30    |  /30   |
|                                                  |      e1/0      |                   To-DPR_DSW2                   |   172.16.3.42    |  /30   |
|                                                  |     Vlan6      |                  Gateway_VLAN6                  |   172.16.3.249   |  /27   |
|                                                  |      Lo1       |                       MNG                       |   192.168.3.97   |  /32   |
|                                                  |                |          Филиал в Запорожской области           |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      ZPR_R1                      |      e0/0      |                  To_Miranda_R1                  |   94.126.24.2    |  /30   |
|                                                  |     e0/1.4     |                  Gateway_VLAN4                  |   192.168.4.65   |  /27   |
|                                                  |     e0/1.6     |                  Gateway_VLAN6                  |  192.168.4.129   |  /27   |
|                                                  |    e0/1.333    |                       MNG                       |    172.16.4.1    |  /29   |
|                                                  |    Tunnel 1    |                                                 |    10.200.0.2    |  /27   |
|                     ZPR_DSW1                     |    Vlan333     |                       MNG                       |    172.16.4.2    |  /29   |
|                     ZPR_ASW1                     |    Vlan333     |                       MNG                       |    172.16.4.3    |  /29   |
|                     ZPR_ASW2                     |    Vlan333     |                       MNG                       |    172.16.4.4    |  /29   |
|                                                  |                |           Филиал в Херсонской области           |                  |        |
|                     Hostname                     |  L3 interface  |                   Description                   |   IPv4-address   |  Mask  |
|                      ZPR_R1                      |      e0/0      |                 To_K-Telecom_R1                 |    46.130.0.2    |  /30   |
|                                                  |     e0/1.4     |                  Gateway_VLAN4                  |   192.168.5.65   |  /27   |
|                                                  |     e0/1.6     |                  Gateway_VLAN6                  |  192.168.5.129   |  /27   |
|                                                  |    e0/1.333    |                       MNG                       |    172.16.5.1    |  /29   |
|                                                  |    Tunnel 1    |                                                 |    10.200.0.5    |  /27   |
|                     ZPR_DSW1                     |    Vlan333     |                       MNG                       |    172.16.5.2    |  /29   |
|                     ZPR_ASW1                     |    Vlan333     |                       MNG                       |    172.16.5.3    |  /29   |
|                     ZPR_ASW2                     |    Vlan333     |                       MNG                       |    172.16.5.4    |  /29   |


#### Анонсируемые префиксы каждой AS на схеме


|       Компании       |  ASN   |            Prefix's             |
|:--------------------:|:------:|:-------------------------------:|
|         ТТК          | 15774  |          5.8.192.0/22           |
|       Мегафон        | 31133  |         85.26.232.0/21          |
|         РТК          | 12389  |         79.133.64.0/24          |
|       Мирнада        | 201776 |         94.126.24.0/21          |
|      К-Телеком       | 43733  |          46.130.0.0/24          |
|        UT-com        | 206810 | 176.96.184.0/22, 31.133.56.0/21 |
|        T-com         | 202279 |         31.133.48.0/21          |
|        L-com         | 43201  |          5.42.216.0/24          |
|        MSK-IX        |  8985  |         195.208.24.0/21         |
| ООО "Туда и обратно" |  1551  |           1.5.51.0/24           |


### Настройка BGP-сессий между провайдерами

Настройка eBGP и iBGP показана на примере провайдера Мегафон AS31133. Внутри автономной
системы роутеры Megafon_R1 и Megafon_R2 связаны по IS-IS и iBGP. Маршруты в сторону пира с 
MSK-IX помечены комюнити 8985:8985 для осуществелния политики фильтрации на RS.

```
 interface Ethernet0/0
 no shutdown
 description To_Megafon_R2
 ip address 85.26.242.1 255.255.255.252
 ip router isis 
!
interface Ethernet0/1
 no shutdown
 no ip address
!
interface Ethernet0/1.333
 no shutdown
 description To_MSK-IX
 encapsulation dot1Q 333
 ip address 195.208.24.3 255.255.248.0
!
interface Ethernet0/2
 no shutdown
 description To_Lugacom_R1
 ip address 85.26.242.5 255.255.255.252
!
interface Ethernet0/3
 no shutdown
 description To_RST_R2
 ip address 1.5.51.6 255.255.255.252
!
router isis
 net 49.0001.3113.3000.0001.00
 redistribute connected
!
router bgp 31133
 no bgp enforce-first-as
 bgp log-neighbor-changes
 neighbor 1.5.51.5 remote-as 1551
 neighbor 85.26.242.2 remote-as 31133
 neighbor 85.26.242.6 remote-as 4321
 neighbor 195.208.24.1 remote-as 8985
 !
 address-family ipv4
  network 85.26.242.0 mask 255.255.255.0
  neighbor 1.5.51.5 activate
  neighbor 1.5.51.5 soft-reconfiguration inbound
  neighbor 85.26.242.2 activate
  neighbor 85.26.242.2 next-hop-self
  neighbor 85.26.242.2 soft-reconfiguration inbound
  neighbor 85.26.242.6 activate
  neighbor 85.26.242.6 soft-reconfiguration inbound
  neighbor 195.208.24.1 activate
  neighbor 195.208.24.1 send-community both
  neighbor 195.208.24.1 soft-reconfiguration inbound
  neighbor 195.208.24.1 route-map RS_Community out
 exit-address-family
!
ip route 85.26.242.0 255.255.255.0 Null0
!
route-map RS_Community permit 10
 set community 8985:8985

```

Проверка:

```
Megafon_R1#sh ip bgp sum

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
1.5.51.5        4         1551       6      11        9    0    0 00:02:08        1
85.26.242.2     4        31133      10      12        9    0    0 00:02:56        3
85.26.242.6     4         4321      11      11        9    0    0 00:02:53        6
195.208.24.1    4         8985      15       8        9    0    0 00:02:36        5

Megafon_R1#sh ip route bgp

      1.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
B        1.5.51.0/24 [20/0] via 1.5.51.5, 00:07:43
      5.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
B        5.8.192.0/22 [20/0] via 195.208.24.2, 00:07:59
B        5.42.216.0/24 [20/0] via 85.26.242.6, 00:08:24
      31.0.0.0/21 is subnetted, 1 subnets
B        31.133.56.0 [20/0] via 195.208.24.4, 00:08:24
      46.0.0.0/24 is subnetted, 1 subnets
B        46.130.0.0 [20/0] via 195.208.24.6, 00:08:17
      79.0.0.0/24 is subnetted, 1 subnets
B        79.133.64.0 [20/0] via 195.208.24.5, 00:08:17
      94.0.0.0/21 is subnetted, 1 subnets
B        94.126.24.0 [20/0] via 195.208.24.5, 00:00:08
      176.96.0.0/21 is subnetted, 1 subnets
B        176.96.184.0 [20/0] via 195.208.24.4, 00:08:24

```

Остальные провайдеры настроены аналогично.

#### Настройка точки обмена трафика

Для орагнизации точки обмена трафиком MSK-IX использован коммутатор (для включения пиров) 
и программный маршрутизатор BIRD (в качестве Route Server).
Настройка BIRD.

- Определение глобальных переменных: ASN и префиксы:
```
define ixp_as = 8985;
define ixp_prefixes = [ 195.208.24.0/21+ ];

template bgp RS_CLIENT {
        local as ixp_as;
        rs client;
}
```

- Описание фильтра для отброса martians-префиксов и собственных:
```
function catch_martians_and_ixp()
prefix set martians;
prefix set ixp_prefixes;
{
  martians = [
  0.0.0.0/8+,
  10.0.0.0/8+,
  100.64.0.0/10+,
  127.0.0.0/8+,
  169.254.0.0/16+,
  172.16.0.0/12+,
  192.0.0.0/24+,
  192.0.2.0/24+,
  192.168.0.0/16+,
  198.18.0.0/15+,
  198.51.100.0/24+,
  203.0.113.0/24+,
  224.0.0.0/4+,
  240.0.0.0/4+ ];

  ixp_prefixes = [ 195.208.24.0/21+ ];

  if net ~ martians || net ~ ixp_prefixes then return true;

  return false;
}
```

- Установка политики маршрутизации: принимать префискы с комьюнити 8985:8985 либо 8985:Х (Х - ASN пира):
```
function bgp_ixp_policy(int peer_as)
{
  if (ixp_as, ixp_as) ~ bgp_community then return true;
  if (ixp_as, peer_as) ~ bgp_community then return true;

  return false;
}

filter reject_martians_and_ixp
{
  if catch_martians_and_ixp() then reject;
  if ( net ~ [0.0.0.0/0{25,32} ] ) then {
    reject;
  }
  accept;
}
```


- Настройка пиринга:

```
protocol bgp as_15774 from RS_CLIENT {
        neighbor 195.208.24.2 as 15774;
        export where bgp_ixp_policy(15774);
        import filter reject_martians_and_ixp;
}

protocol bgp as_31133 from RS_CLIENT {
        neighbor 195.208.24.3 as 31133;
        export where bgp_ixp_policy(31133);
        import filter reject_martians_and_ixp;
}

protocol bgp as_26810 from RS_CLIENT {
        neighbor 195.208.24.4 as 26810;
        export where bgp_ixp_policy(26810);
        import filter reject_martians_and_ixp;
}

protocol bgp as_12389 from RS_CLIENT {
        neighbor 195.208.24.5 as 12389;
        export where bgp_ixp_policy(12389);
        import filter reject_martians_and_ixp;
}

protocol bgp as_43733 from RS_CLIENT {
        neighbor 195.208.24.6 as 43733;
        export where bgp_ixp_policy(43733);
        import filter reject_martians_and_ixp;
}

```

Проверка работоспособности:

``` 
bird> show protocols "as*"
name     proto    table    state  since       info
as_15774 BGP      master   up     18:29:43    Established
as_31133 BGP      master   up     18:29:39    Established
as_26810 BGP      master   up     18:29:43    Established
as_12389 BGP      master   up     18:29:43    Established
as_43733 BGP      master   up     18:29:42    Established

bird> show route
0.0.0.0/0          via 172.16.0.10 on eth1 [kernel1 18:29:38] * (10)
79.133.64.0/24     via 195.208.24.5 on eth0 [as_12389 18:30:35] * (100) [AS12389i]
                   via 195.208.24.4 on eth0 [as_26810 18:30:57] (100) [AS12389i]
1.5.51.0/24        via 195.208.24.3 on eth0 [as_31133 18:31:29] * (100) [AS1551i]
                   via 195.208.24.4 on eth0 [as_26810 18:31:27] (100) [AS1551i]
46.130.0.0/24      via 195.208.24.6 on eth0 [as_43733 18:30:36] * (100) [AS43733i]
31.133.56.0/21     via 195.208.24.4 on eth0 [as_26810 18:30:27] * (100) [AS26810i]
169.254.0.0/16     dev eth0 [kernel1 18:29:42] * (10)
5.42.216.0/24      via 195.208.24.3 on eth0 [as_31133 18:30:27] * (100) [AS4321i]
                   via 195.208.24.4 on eth0 [as_26810 18:30:27] (100) [AS4321i]
176.96.184.0/21    via 195.208.24.4 on eth0 [as_26810 18:30:27] * (100) [AS26810i]
195.208.24.0/21    dev eth0 [direct1 18:29:38] * (240)
94.126.24.0/21     via 195.208.24.5 on eth0 [as_12389 18:38:43] * (100) [AS201776i]
                   via 195.208.24.4 on eth0 [as_26810 18:38:43] (100) [AS201776i]
85.26.242.0/24     via 195.208.24.3 on eth0 [as_31133 18:30:27] * (100) [AS31133i]
                   via 195.208.24.4 on eth0 [as_26810 18:30:27] (100) [AS31133i]
5.8.192.0/22       via 195.208.24.2 on eth0 [as_15774 18:30:53] * (100) [AS15774i]

```

### Настройка сетей центрального офиса и удалённых филиалов

Для сегментации сети использованы VLAN и разные подсети.

| VLAN_ID |   Description   |                  |                  |        Network            |                     |                    |
|:-------:|:---------------:|:----------------:|:----------------:|:----------------:|:-------------------:|:------------------:|
|         |                 |  Ростов-на-Дону  | ЛНР              | ДНР              | Запорожская область | Херсонская область |
| 2       | Administration  |  192.168.1.0/27  | 192.168.2.0/27   | 192.168.3.0/27   |                     |                    |
| 3       |   Buhgalteria   | 192.168.1.32/27  | 192.168.2.32/27  | 192.168.3.32/27  |                     |                    |
| 4       |      Sklad      | 192.168.1.64/27  | 192.168.2.64/27  | 192.168.3.64/27  |   192.168.4.64/27   |  192.168.4.64/27   |
| 5       |       ATP       | 192.168.1.96/27  |                  |                  |                     |                    |
| 6       | SB \|\| Okhrana | 192.168.1.128/27 | 192.168.2.128/27 | 192.168.3.128/27 |  192.168.4.128/27   |  192.168.4.128/27  |
| 7       |       IT        | 192.168.1.160/27 |

Сети p2p:
Ростов-на-Дону - 172.16.1.0/24 (дробиться по /30)
ЛНР - 172.16.2.0/24 (дробиться по /30)
ДНР - 172.16.3.0/24 (дробиться по /30)


Менеджмент построен на loopback-интерфейсах либо VLAN-интерфейсах (свичи l2).
Для раздачи адресов конечным устройствам используется протокол DHCP.

#### Настройка сети центрального офиса в Ростове-на-Дону

Схема сети центрального офиса в Ростове-на-Дону


![RST.png](img%2FRST.png)


Внутренняя маршрутизация построена на протоколе OSPF. Как видно из схемы, сеть разбита на
зоны:
* Area 0 - Backbone
* Area 1 (Stub) - уровень доступа и дистрибуции

Вывод команды show ip ospf database (LSDB):
```
RST_R1#sh ip ospf database

            OSPF Router with ID (1.1.1.1) (Process ID 110)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         1086        0x80000003 0x0038E1 7
2.2.2.2         2.2.2.2         1089        0x80000003 0x00F5AB 7
3.3.3.3         3.3.3.3         1092        0x80000004 0x00D5ED 7
4.4.4.4         4.4.4.4         1093        0x80000005 0x00E8D1 7
8.8.8.8         8.8.8.8         1089        0x80000005 0x00208E 9

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
172.16.1.28     3.3.3.3         1194        0x80000001 0x00D778
172.16.1.28     4.4.4.4         1108        0x80000002 0x00C188
172.16.1.32     3.3.3.3         1194        0x80000001 0x00AF9C
172.16.1.32     4.4.4.4         1177        0x80000002 0x00F349
172.16.1.36     3.3.3.3         1194        0x80000001 0x0087C0
172.16.1.36     4.4.4.4         1187        0x80000001 0x00CD6C
172.16.1.40     3.3.3.3         1097        0x80000002 0x0067DA
172.16.1.40     4.4.4.4         1197        0x80000001 0x0041FE
172.16.1.44     3.3.3.3         1184        0x80000001 0x009B9A
172.16.1.44     4.4.4.4         1197        0x80000001 0x001923
172.16.1.48     3.3.3.3         1184        0x80000001 0x0073BE
172.16.1.48     4.4.4.4         1197        0x80000001 0x00F047
172.16.1.248    3.3.3.3         1184        0x80000001 0x00531C
172.16.1.248    4.4.4.4         1187        0x80000001 0x003536
172.16.1.249    3.3.3.3         1184        0x80000001 0x004925
172.16.1.249    4.4.4.4         1177        0x80000001 0x002B3F
172.16.1.250    3.3.3.3         1097        0x80000002 0x003D2F
172.16.1.250    4.4.4.4         1109        0x80000001 0x002148
192.168.1.0     3.3.3.3         1097        0x80000002 0x0024B5
192.168.1.0     4.4.4.4         1108        0x80000001 0x0008CE
192.168.1.32    3.3.3.3         1097        0x80000002 0x00E2D6
192.168.1.32    4.4.4.4         1108        0x80000001 0x00C6EF
192.168.1.64    3.3.3.3         1184        0x80000001 0x00A3F6
192.168.1.64    4.4.4.4         1177        0x80000001 0x008511
192.168.1.96    3.3.3.3         1184        0x80000001 0x006218
192.168.1.96    4.4.4.4         1177        0x80000001 0x004432
192.168.1.128   3.3.3.3         1184        0x80000001 0x002139
192.168.1.128   4.4.4.4         1187        0x80000001 0x000353
192.168.1.160   3.3.3.3         1184        0x80000001 0x00DF5A
192.168.1.160   4.4.4.4         1187        0x80000001 0x00C174

                Type-5 AS External Link States

Link ID         ADV Router      Age         Seq#       Checksum Tag
0.0.0.0         1.1.1.1         1097        0x80000001 0x00CE72 110
10.200.0.0      2.2.2.2         1093        0x80000001 0x0046B1 0
172.16.2.0      2.2.2.2         997         0x80000001 0x003FB0 0
172.16.2.4      2.2.2.2         997         0x80000001 0x0017D4 0
172.16.2.8      2.2.2.2         997         0x80000001 0x00EEF8 0
172.16.2.12     2.2.2.2         997         0x80000001 0x00C61D 0
172.16.2.16     2.2.2.2         997         0x80000001 0x009E41 0
172.16.2.20     2.2.2.2         997         0x80000001 0x007665 0
172.16.2.250    2.2.2.2         997         0x80000001 0x00836E 0
172.16.2.251    2.2.2.2         997         0x80000001 0x007977 0
172.16.2.252    2.2.2.2         997         0x80000001 0x006F80 0
172.16.2.253    2.2.2.2         997         0x80000001 0x006589 0
172.16.2.254    2.2.2.2         997         0x80000001 0x005B92 0
172.16.3.0      2.2.2.2         985         0x80000001 0x0034BA 0
172.16.3.4      2.2.2.2         985         0x80000001 0x000CDE 0
172.16.3.8      2.2.2.2         985         0x80000001 0x00E303 0
172.16.3.12     2.2.2.2         985         0x80000001 0x00BB27 0
172.16.3.16     2.2.2.2         985         0x80000001 0x00934B 0
172.16.3.20     2.2.2.2         985         0x80000001 0x006B6F 0
172.16.3.24     2.2.2.2         985         0x80000001 0x004393 0
172.16.3.28     2.2.2.2         985         0x80000001 0x001BB7 0
172.16.3.32     2.2.2.2         985         0x80000001 0x00F2DB 0
172.16.3.36     2.2.2.2         985         0x80000001 0x00CAFF 0
172.16.3.40     2.2.2.2         985         0x80000001 0x00A224 0
172.16.3.248    2.2.2.2         985         0x80000001 0x008C66 0
172.16.3.249    2.2.2.2         985         0x80000001 0x00826F 0
172.16.3.250    2.2.2.2         985         0x80000001 0x007878 0
172.16.3.251    2.2.2.2         985         0x80000001 0x006E81 0
172.16.3.252    2.2.2.2         985         0x80000001 0x00648A 0
172.16.3.253    2.2.2.2         985         0x80000001 0x005A93 0
172.16.3.254    2.2.2.2         985         0x80000001 0x00509C 0
172.16.4.0      2.2.2.2         545         0x80000001 0x0011E0 0
172.16.5.0      2.2.2.2         998         0x80000001 0x0006EA 0
192.168.2.0     2.2.2.2         997         0x80000001 0x006AF4 0
192.168.2.32    2.2.2.2         997         0x80000001 0x002916 0
192.168.2.64    2.2.2.2         997         0x80000001 0x00E737 0
192.168.2.96    2.2.2.2         997         0x80000001 0x00A658 0
192.168.3.0     2.2.2.2         985         0x80000001 0x005FFE 0
192.168.3.32    2.2.2.2         985         0x80000001 0x001E20 0
192.168.3.64    2.2.2.2         985         0x80000001 0x00DC41 0
192.168.3.96    2.2.2.2         985         0x80000001 0x009B62 0
192.168.4.64    2.2.2.2         545         0x80000001 0x00D14B 0
192.168.4.128   2.2.2.2         545         0x80000001 0x004F8D 0
192.168.5.64    2.2.2.2         998         0x80000001 0x00C655 0
192.168.5.128   2.2.2.2         998         0x80000001 0x004497 0
```

Подняты BGP-сессии с ТТК и Мегафон. Использую препенды и Local Preference
приоритетным провайдером сделан Мегафон. Чтоб AS1551 не стала транзитной, применен
соответствующий фильтр is_transit.

RST_R1
```
router bgp 1551
 bgp log-neighbor-changes
 neighbor 1.5.51.2 remote-as 15774
 neighbor 172.16.1.253 remote-as 1551
 neighbor 172.16.1.253 update-source Loopback1
 !
 address-family ipv4
  network 1.5.51.0 mask 255.255.255.0
  neighbor 1.5.51.2 activate
  neighbor 1.5.51.2 soft-reconfiguration inbound
  neighbor 1.5.51.2 route-map To-TTK_R1 out
  neighbor 1.5.51.2 filter-list 1 out
  neighbor 172.16.1.253 activate
  neighbor 172.16.1.253 next-hop-self
  neighbor 172.16.1.253 soft-reconfiguration inbound
 exit-address-family
!
route-map To-TTK_R1 permit 5
 set as-path prepend 1551 1551 1551
!
ip as-path access-list 1 permit ^$
```

RST_R2
```
router bgp 1551
 bgp log-neighbor-changes
 neighbor 1.5.51.6 remote-as 31133
 neighbor 172.16.1.254 remote-as 1551
 neighbor 172.16.1.254 update-source Loopback1
 !
 address-family ipv4
  network 1.5.51.0 mask 255.255.255.0
  neighbor 1.5.51.6 activate
  neighbor 1.5.51.6 soft-reconfiguration inbound
  neighbor 1.5.51.6 route-map To-Megafon_R1 in
  neighbor 1.5.51.6 filter-list 1 out
  neighbor 172.16.1.254 activate
  neighbor 172.16.1.254 next-hop-self
  neighbor 172.16.1.254 soft-reconfiguration inbound
 exit-address-family
!
route-map To-Megafon_R1 permit 5
 set local-preference 150
!
ip as-path access-list 1 permit ^$
```
Проверка на RST_R1

```
RST_R1#sh ip bgp sum

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
1.5.51.2        4        15774      37      31       11    0    0 00:25:05        8
172.16.1.253    4         1551      38      31       11    0    0 00:24:51        9

RST_R1#sh bgp ipv4 unicast neighbors 1.5.51.2 advertised-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *>  1.5.51.0/24      0.0.0.0                  0         32768 i

Total number of prefixes 1

RST_R1#sh ip route bgp

      5.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
B        5.8.192.0/22 [200/0] via 172.16.1.253, 00:25:12
B        5.42.216.0/24 [200/0] via 172.16.1.253, 00:25:12
      31.0.0.0/21 is subnetted, 1 subnets
B        31.133.56.0 [200/0] via 172.16.1.253, 00:25:12
      46.0.0.0/24 is subnetted, 1 subnets
B        46.130.0.0 [200/0] via 172.16.1.253, 00:25:12
      79.0.0.0/24 is subnetted, 1 subnets
B        79.133.64.0 [200/0] via 172.16.1.253, 00:25:12
      85.0.0.0/24 is subnetted, 1 subnets
B        85.26.242.0 [200/0] via 172.16.1.253, 00:25:12
      94.0.0.0/21 is subnetted, 1 subnets
B        94.126.24.0 [200/0] via 172.16.1.253, 00:17:07
      176.96.0.0/21 is subnetted, 1 subnets
B        176.96.184.0 [200/0] via 172.16.1.253, 00:25:12

```

Проверка на RST_R2
```
RST_R2#sh ip bgp sum

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
1.5.51.6        4        31133      40      33       11    0    0 00:26:58        8
172.16.1.254    4         1551      33      40       11    0    0 00:26:51        1

RST_R2#sh bgp ipv4 unicast neighbors 1.5.51.6 advertised-routes

     Network          Next Hop            Metric LocPrf Weight Path
 *>  1.5.51.0/24      0.0.0.0                  0         32768 i

Total number of prefixes 1

RST_R2#sh ip route bgp

      5.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
B        5.8.192.0/22 [20/0] via 1.5.51.6, 00:27:22
B        5.42.216.0/24 [20/0] via 1.5.51.6, 00:27:22
      31.0.0.0/21 is subnetted, 1 subnets
B        31.133.56.0 [20/0] via 1.5.51.6, 00:27:22
      46.0.0.0/24 is subnetted, 1 subnets
B        46.130.0.0 [20/0] via 1.5.51.6, 00:27:22
      79.0.0.0/24 is subnetted, 1 subnets
B        79.133.64.0 [20/0] via 1.5.51.6, 00:27:22
      85.0.0.0/24 is subnetted, 1 subnets
B        85.26.242.0 [20/0] via 1.5.51.6, 00:27:22
      94.0.0.0/21 is subnetted, 1 subnets
B        94.126.24.0 [20/0] via 1.5.51.6, 00:19:17
      176.96.0.0/21 is subnetted, 1 subnets
B        176.96.184.0 [20/0] via 1.5.51.6, 00:27:22

```

Также на маршрутизаторах RST_R1, RST_R2 настроен NAT overload:

```
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
!
ip access-list standard NAT_Hosts
 permit 192.168.1.0 0.0.0.255
 deny   any
```
В качестве NTP-сервера выступает
RST_DHCP.

#### Настройка сети филиала в Луганской Народной Республике

Схема сети филиала в ЛНР


![LPR.png](img%2FLPR.png)

Внутрення маршрутизация построена с помощью протокола OSPF. Все сетевые устройства находятся
в Area 0.

Вывод команды show ip ospf database (LSDB):

```
LPR_R1#sh ip ospf database

            OSPF Router with ID (1.1.1.1) (Process ID 110)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         265         0x80000005 0x003E72 3
2.2.2.2         2.2.2.2         264         0x8000000A 0x0057E8 9
3.3.3.3         3.3.3.3         273         0x80000007 0x00A9B4 6
4.4.4.4         4.4.4.4         272         0x80000007 0x00ADD1 9
5.5.5.5         5.5.5.5         270         0x80000009 0x0031E4 6

                Type-5 AS External Link States

Link ID         ADV Router      Age         Seq#       Checksum Tag
0.0.0.0         1.1.1.1         286         0x80000004 0x00C875 110

```

Вывод команды show ip route ospf:

```
LPR_R1#sh ip route ospf

      172.16.0.0/16 is variably subnetted, 51 subnets, 3 masks
O        172.16.2.4/30 [110/20] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.8/30 [110/20] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.12/30 [110/20] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.16/30 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.20/30 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.250/32 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.251/32 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.252/32 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        172.16.2.253/32 [110/11] via 172.16.2.2, 00:05:08, Ethernet0/1
      192.168.2.0/27 is subnetted, 4 subnets
O        192.168.2.0 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        192.168.2.32 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        192.168.2.64 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
O        192.168.2.128 [110/21] via 172.16.2.2, 00:05:08, Ethernet0/1
```

NAT настроен на LPR_R1:
```
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
!
ip access-list standard NAT_Hosts
 permit 192.168.2.0 0.0.0.255
 deny   any
```

NTP-сервером является LPR_DSW1.


#### Настройка сети филиала в Донецкой Народной Республике

Схема сети филиала в ДНР

![DPR.png](img%2FDPR.png)

Внутренняя маршрутизация построена на протоколе OSPF. Как видно из схемы, сеть разбита на
зоны:
* Area 0 - Backbone
* Area 1 (Stub) - уровень доступа и дистрибуции

Таблица LSDB на роутере DPR_R1 выглядит следующим образом:

```
DPR_R1#sh ip ospf database

            OSPF Router with ID (1.1.1.1) (Process ID 110)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         853         0x80000008 0x000902 7
2.2.2.2         2.2.2.2         857         0x80000007 0x006361 5
3.3.3.3         3.3.3.3         859         0x80000006 0x00B1FC 5
7.7.7.7         7.7.7.7         854         0x8000000A 0x00CBF9 7

                Summary Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
172.16.3.20     2.2.2.2         857         0x80000004 0x002A2D
172.16.3.20     3.3.3.3         859         0x80000005 0x00143D
172.16.3.24     2.2.2.2         857         0x80000004 0x000251
172.16.3.24     3.3.3.3         859         0x80000004 0x0048FC
172.16.3.28     2.2.2.2         857         0x80000004 0x00D975
172.16.3.28     3.3.3.3         859         0x80000004 0x002021
172.16.3.32     2.2.2.2         857         0x80000005 0x00B98F
172.16.3.32     3.3.3.3         859         0x80000004 0x0093B3
172.16.3.36     2.2.2.2         843         0x80000006 0x00E951
172.16.3.36     3.3.3.3         859         0x80000004 0x006BD7
172.16.3.40     2.2.2.2         843         0x80000006 0x00C175
172.16.3.40     3.3.3.3         859         0x80000004 0x0043FB
172.16.3.249    2.2.2.2         843         0x80000006 0x004724
172.16.3.249    3.3.3.3         859         0x80000004 0x002D3C
172.16.3.250    2.2.2.2         843         0x80000006 0x003D2D
172.16.3.250    3.3.3.3         859         0x80000004 0x002345
172.16.3.251    2.2.2.2         857         0x80000004 0x003734
172.16.3.251    3.3.3.3         859         0x80000004 0x00194E
192.168.3.0     2.2.2.2         857         0x80000004 0x0028B1
192.168.3.0     3.3.3.3         859         0x80000004 0x000ACB
192.168.3.32    2.2.2.2         857         0x80000004 0x00E6D2
192.168.3.32    3.3.3.3         859         0x80000004 0x00C8EC
192.168.3.64    2.2.2.2         843         0x80000006 0x00A1F5
192.168.3.64    3.3.3.3         859         0x80000004 0x00870E
192.168.3.128   2.2.2.2         843         0x80000005 0x002137
192.168.3.128   3.3.3.3         859         0x80000003 0x00074F

                Type-5 AS External Link States

Link ID         ADV Router      Age         Seq#       Checksum Tag
0.0.0.0         1.1.1.1         880         0x80000004 0x00C875 110
```

Вывод команды show ip route ospf:
```
DPR_R1#show ip route ospf

      172.16.0.0/16 is variably subnetted, 54 subnets, 3 masks
O        172.16.3.12/30 [110/20] via 172.16.3.10, 00:15:01, Ethernet0/2
                        [110/20] via 172.16.3.2, 00:15:01, Ethernet0/1
O        172.16.3.16/30 [110/20] via 172.16.3.10, 00:15:01, Ethernet0/2
                        [110/20] via 172.16.3.6, 00:15:01, Ethernet1/1
O IA     172.16.3.20/30 [110/20] via 172.16.3.2, 00:15:01, Ethernet0/1
O IA     172.16.3.24/30 [110/20] via 172.16.3.2, 00:15:01, Ethernet0/1
O IA     172.16.3.28/30 [110/20] via 172.16.3.2, 00:15:01, Ethernet0/1
O IA     172.16.3.32/30 [110/20] via 172.16.3.6, 00:15:01, Ethernet1/1
O IA     172.16.3.36/30 [110/20] via 172.16.3.6, 00:15:01, Ethernet1/1
O IA     172.16.3.40/30 [110/20] via 172.16.3.6, 00:15:01, Ethernet1/1
O        172.16.3.248/32 [110/11] via 172.16.3.10, 00:15:01, Ethernet0/2
O IA     172.16.3.249/32 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                         [110/21] via 172.16.3.2, 00:14:55, Ethernet0/1
O IA     172.16.3.250/32 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                         [110/21] via 172.16.3.2, 00:14:55, Ethernet0/1
O IA     172.16.3.251/32 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                         [110/21] via 172.16.3.2, 00:15:01, Ethernet0/1
O        172.16.3.252/32 [110/11] via 172.16.3.6, 00:15:01, Ethernet1/1
O        172.16.3.253/32 [110/11] via 172.16.3.2, 00:15:01, Ethernet0/1
      192.168.3.0/27 is subnetted, 4 subnets
O IA     192.168.3.0 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                     [110/21] via 172.16.3.2, 00:15:01, Ethernet0/1
O IA     192.168.3.32 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                      [110/21] via 172.16.3.2, 00:15:01, Ethernet0/1
O IA     192.168.3.64 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                      [110/21] via 172.16.3.2, 00:14:55, Ethernet0/1
O IA     192.168.3.128 [110/21] via 172.16.3.6, 00:15:01, Ethernet1/1
                       [110/21] via 172.16.3.2, 00:14:55, Ethernet0/1
```

NAT настроен на интерфейсе Loopback0:
```
interface Loopback0
 description For_NAT
 ip address 31.133.48.13 255.255.255.252 secondary
 ip address 176.96.184.13 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
ip nat inside source list NAT_Hosts interface Loopback0 overload
```

Данный офис имеет 2 аплинка на разных провайдером, основным выбран UT-com (c помощью указания метрик
в статических маршрутах). Используется SLA:

```
track 1 ip sla 1 reachability
 delay down 10 up 10
!
ip route 0.0.0.0 0.0.0.0 176.96.184.5 50 track 1
ip route 0.0.0.0 0.0.0.0 31.133.48.1 100
!
ip sla 1
 icmp-echo 176.96.184.5 source-ip 176.96.184.6
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 1 life forever start-time now
```

NTP-сервером выступает маршрутизатор DPR_DHCP.

#### Настройка сети филиалов в Заопрожской и Херсонской областях

Сети офисов в Запорожской и Херсонской областях имеют идентичню структуру и построены по принципу
Router-on-a-Stick. Основные настройки находятся на пограничных маршрутизаторах, которые также выступают
шлюзом для внутренних сетей, DHCP и NTP-сервером. 

ZPR_R1:

```
ip dhcp excluded-address 192.168.4.65
ip dhcp excluded-address 192.168.4.129
!
ip dhcp pool Sklad
 network 192.168.4.64 255.255.255.224
 default-router 192.168.4.65
 dns-server 192.168.4.65
!
ip dhcp pool SB
 network 192.168.4.128 255.255.255.224
 default-router 192.168.4.129
 dns-server 192.168.4.129
!
interface Ethernet0/0
 description To_Miranda_R1
 ip address 94.126.24.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 no ip address
!
interface Ethernet0/1.4
 description Gateway_VLAN4
 encapsulation dot1Q 4
 ip address 192.168.4.65 255.255.255.224
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1.6
 description Gateway_VLAN6
 encapsulation dot1Q 6
 ip address 192.168.4.129 255.255.255.224
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1.333
 description MNG
 encapsulation dot1Q 333
 ip address 172.16.4.1 255.255.255.248
!
interface Ethernet0/1.777
 encapsulation dot1Q 777 native
!
ip nat inside source list NAT_Hosts interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 94.126.24.1
!
ip access-list standard NAT_Hosts
 permit 192.168.4.0 0.0.0.255
 deny   any
!
ntp master 1
ntp update-calendar
```

### Достижение IP-связности всех узлов компании с помощью DMVPN


Для настройки туннелей взята сеть 10.200.0.0/27. Чтоб организовать связность
внутренних сетей офисов, интерфейсы туннелей (Tunnel 1) добавлены в процесс RIP и сделана
редистрибьюция маршрутов из OSPF (Ростов-на-Дону, ЛНР, ДНР) и connected (Запорожская и
Херсонская области). 


Настройка Tunnel 1 на RST_R1:
```
interface Loopback0
 ip address 1.5.51.254 255.255.255.255
!
interface Tunnel1
 ip address 10.200.0.1 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map multicast dynamic
 ip nhrp network-id 200
 no ip split-horizon
 ip tcp adjust-mss 1400
 tunnel source Loopback0
 tunnel mode gre multipoint
!
router rip
 version 2
 redistribute ospf 110 metric 5
 network 10.0.0.0
 no auto-summary
```


Настройка Tunnel 1 на ZPR_R1:
```
interface Tunnel1
 ip address 10.200.0.2 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 description To_Miranda_R1
 ip address 94.126.24.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
router rip
 version 2
 redistribute connected route-map RC
 network 10.0.0.0
 no auto-summary
```


Настройка Tunnel 1 на LPR_R1:
```
interface Tunnel1
 ip address 10.200.0.3 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 description To_Lugacom_R1
 ip address 5.42.216.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
router rip
 version 2
 redistribute ospf 110 metric 2
 network 10.0.0.0
 no auto-summary
```


Настройка Tunnel 1 на DPR_R1:
```
interface Loopback0
 description For_NAT
 ip address 31.133.48.13 255.255.255.252 secondary
 ip address 176.96.184.13 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Tunnel1
 ip address 10.200.0.4 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Loopback0
 tunnel mode gre multipoint
!
router rip
 version 2
 redistribute ospf 110 metric 2
 network 10.0.0.0
 no auto-summary
```


Настройка Tunnel 1 на HRS_R1:
```
interface Tunnel1
 ip address 10.200.0.5 255.255.255.224
 no ip redirects
 ip mtu 1500
 ip nhrp map 10.200.0.1 1.5.51.254
 ip nhrp map multicast 1.5.51.254
 ip nhrp network-id 200
 ip nhrp nhs 10.200.0.1
 ip nhrp registration no-unique
 ip tcp adjust-mss 1400
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 description To_K-Telecom_R1
 ip address 46.130.0.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
router rip
 version 2
 redistribute connected route-map RC
 network 10.0.0.0
 no auto-summary
```


Проверка состояния туннелей на RST_R2:

```
RST_R2#sh dmvpn

Interface: Tunnel1, IPv4 NHRP Details
Type:Hub, NHRP Peers:4,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 94.126.24.2          10.200.0.2    UP 00:00:09     D
     1 5.42.216.2           10.200.0.3    UP 00:47:34     D
     1 176.96.184.13        10.200.0.4    UP 00:47:41     D
     1 46.130.0.2           10.200.0.5    UP 00:47:32     D
```

Проверка доступности по ICMP:

```
RST_R2#ping 10.200.0.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.200.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/3/4 ms
RST_R2#ping 10.200.0.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.200.0.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/2 ms
RST_R2#ping 10.200.0.4
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.200.0.4, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
RST_R2#ping 10.200.0.5
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.200.0.5, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
```

Просмотр табицы маршрутизации (протокол RIP) на RST_R2:
```
RST_R2#sh ip route rip
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      172.16.0.0/16 is variably subnetted, 55 subnets, 3 masks
R        172.16.2.0/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.4/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.8/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.12/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.16/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.20/30 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.250/32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.251/32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.252/32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.253/32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.2.254/32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        172.16.3.0/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.4/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.8/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.12/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.16/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.20/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.24/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.28/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.32/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.36/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.40/30 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.248/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.249/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.250/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.251/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.252/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.253/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.3.254/32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        172.16.4.0/29 [120/1] via 10.200.0.2, 00:00:19, Tunnel1
R        172.16.5.0/29 [120/1] via 10.200.0.5, 00:00:16, Tunnel1
      192.168.2.0/27 is subnetted, 4 subnets
R        192.168.2.0 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        192.168.2.32 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        192.168.2.64 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
R        192.168.2.128 [120/2] via 10.200.0.3, 00:00:14, Tunnel1
      192.168.3.0/27 is subnetted, 4 subnets
R        192.168.3.0 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        192.168.3.32 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        192.168.3.64 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
R        192.168.3.128 [120/2] via 10.200.0.4, 00:00:03, Tunnel1
      192.168.4.0/27 is subnetted, 2 subnets
R        192.168.4.64 [120/1] via 10.200.0.2, 00:00:19, Tunnel1
R        192.168.4.128 [120/1] via 10.200.0.2, 00:00:19, Tunnel1
      192.168.5.0/27 is subnetted, 2 subnets
R        192.168.5.64 [120/1] via 10.200.0.5, 00:00:16, Tunnel1
R        192.168.5.128 [120/1] via 10.200.0.5, 00:00:16, Tunnel1
```

Для проверки связности внутренних сетей офисов используем ping  и трассировку с хоста 
администрации офиса в Ростове-на-Дону на хосты филиалов:


```
Admin> ip dhcp
DDORA IP 192.168.1.2/27 GW 192.168.1.1

Admin> ping 192.168.2.66

84 bytes from 192.168.2.66 icmp_seq=1 ttl=58 time=16.428 ms
84 bytes from 192.168.2.66 icmp_seq=2 ttl=58 time=9.163 ms
84 bytes from 192.168.2.66 icmp_seq=3 ttl=58 time=7.846 ms
84 bytes from 192.168.2.66 icmp_seq=4 ttl=58 time=10.548 ms
84 bytes from 192.168.2.66 icmp_seq=5 ttl=58 time=11.535 ms

Admin> trace 192.168.2.66
trace to 192.168.2.66, 8 hops max, press Ctrl+C to stop
 1   192.168.1.1   4.780 ms  4.043 ms  5.501 ms
 2   172.16.1.41   2.704 ms  3.881 ms  3.376 ms
 3   172.16.1.17   4.329 ms  2.946 ms  3.495 ms
 4   10.200.0.3   4.946 ms  6.586 ms  5.372 ms
 5   172.16.2.2   5.362 ms  5.633 ms  5.906 ms
 6   172.16.2.10   9.607 ms  13.696 ms  8.626 ms
 7   *192.168.2.66   7.095 ms (ICMP type:3, code:3, Destination port unreachable)

Admin> ping 192.168.3.34

84 bytes from 192.168.3.34 icmp_seq=1 ttl=58 time=15.479 ms
84 bytes from 192.168.3.34 icmp_seq=2 ttl=58 time=9.558 ms
84 bytes from 192.168.3.34 icmp_seq=3 ttl=58 time=8.603 ms
84 bytes from 192.168.3.34 icmp_seq=4 ttl=58 time=13.433 ms
84 bytes from 192.168.3.34 icmp_seq=5 ttl=58 time=10.287 ms

Admin> trace 192.168.3.34
trace to 192.168.3.34, 8 hops max, press Ctrl+C to stop
 1   192.168.1.1   3.655 ms  4.321 ms  5.483 ms
 2   172.16.1.41   3.114 ms  3.786 ms  3.436 ms
 3   172.16.1.17   4.015 ms  3.541 ms  4.540 ms
 4   10.200.0.4   7.905 ms  6.476 ms  11.581 ms
 5   172.16.3.2   6.462 ms  8.507 ms  7.349 ms
 6   172.16.3.22   12.778 ms  12.728 ms  13.461 ms
 7   *192.168.3.34   15.266 ms (ICMP type:3, code:3, Destination port unreachable)

Admin> ping 192.168.4.66

84 bytes from 192.168.4.66 icmp_seq=1 ttl=60 time=12.018 ms
84 bytes from 192.168.4.66 icmp_seq=2 ttl=60 time=7.157 ms
84 bytes from 192.168.4.66 icmp_seq=3 ttl=60 time=7.642 ms
84 bytes from 192.168.4.66 icmp_seq=4 ttl=60 time=6.902 ms
84 bytes from 192.168.4.66 icmp_seq=5 ttl=60 time=8.814 ms

Admin> trace 192.168.4.66
trace to 192.168.4.66, 8 hops max, press Ctrl+C to stop
 1   192.168.1.1   3.143 ms  4.353 ms  2.680 ms
 2   172.16.1.41   2.050 ms  2.904 ms  3.067 ms
 3   172.16.1.17   6.280 ms  3.035 ms  3.163 ms
 4   10.200.0.2   6.465 ms  6.140 ms  7.475 ms
 5   *192.168.4.66   7.521 ms (ICMP type:3, code:3, Destination port unreachable)

Admin> ping 192.168.5.130

84 bytes from 192.168.5.130 icmp_seq=1 ttl=60 time=11.159 ms
84 bytes from 192.168.5.130 icmp_seq=2 ttl=60 time=6.188 ms
84 bytes from 192.168.5.130 icmp_seq=3 ttl=60 time=13.065 ms
84 bytes from 192.168.5.130 icmp_seq=4 ttl=60 time=6.738 ms
84 bytes from 192.168.5.130 icmp_seq=5 ttl=60 time=5.998 ms

Admin> trace 192.168.5.130
trace to 192.168.5.130, 8 hops max, press Ctrl+C to stop
 1   192.168.1.1   4.934 ms  2.646 ms  3.805 ms
 2   172.16.1.41   5.070 ms  2.354 ms  3.118 ms
 3   172.16.1.17   4.686 ms  4.224 ms  3.873 ms
 4   10.200.0.5   6.606 ms  6.292 ms  7.463 ms
 5   *192.168.5.130   7.963 ms (ICMP type:3, code:3, Destination port unreachable)
```

Таким образом IP-связность сетей офисов установлена.