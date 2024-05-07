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

!Надо откорректировать!

| Компании             | ASN    | Prefix's                         |
|:--------------------:|:------:|:--------------------------------:|
| ТТК                  | 15774  | 5.8.192.0/22, 188.168.0.0/16     |
| Мегафон              | 31133  | 85.26.232.0/21, 109.106.192.0/19 |
| РТК                  | 12389  | 79.133.64.0/19, 213.242.0.0/18   |
| Мирнада              | 201776 | 178.34.191.0/24, 94.126.24.0/21  |
| К-Телеком            | 43733  | 46.130.0.0/16, 217.76.0.0/20     |
| Углетелеком          | 206810 | 176.96.184.0/22, 31.40.157.0/24  |
| Комтел               | 202279 | 31.133.48.0/21, 128.0.80.0/22    |
| Лугаком              | 43201  | 5.42.216.0/24, 195.158.220.0/22  |
| MSK-IX               | 8985   | 195.208.24.0/21                  |
| ООО "Туда и обратно" | 1551   | 1.5.51.0/24                      |


### Настройка BGP-сессий между провайдерами

Настройка eBGP и iBGP показана на примере провайдера Мегафон AS31133.

eBGP:
``` ```

iBGP:
``` ```

Анонс сетей AS31133:

``` ```

Проверка:

``` ```

Остальные провайдеры настроены аналогично.

#### Настройка точки обмена трафика

Для орагнизации точки обмена трафиком MSK-IX использован коммутатор (для включения пиров) и программный маршрутизатор BIRD (в качестве Route Server).
СТАТЬЯ ИЗ ХАБА
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


- Настройка пиринга (НАДО ИЗМЕНИТЬ):

```
protocol as_100 from RS_CLIENT {
neighbor 50.50.50.10 as 100;
ipv4 {
export where bgp_ixp_policy(100);
import filter reject_martians_and_ixp;
}
}

protocol as_200 from RS_CLIENT {
neighbor 50.50.50.20 as 200;
ipv4 {
export where bgp_ixp_policy(200);
import filter reject_martians_and_ixp;
}
}

protocol as_300 from RS_CLIENT {
neighbor 50.50.50.30 as 300;
ipv4 {
export where bgp_ixp_policy(300);
import filter reject_martians_and_ixp;
}
}
```

Проверка работоспособности:

``` ```

### Настройка сетей центрального офиса и удалённых филиалов

#### Настройка сети центрального офиса в Ростове-на-Дону

Внутренняя маршрутизация построена на протоколе OSPF. Как видно из схемы, сеть разбита на зоны:
* Area 0 - Backbone
* Area 1 (Stub) - уровень доступа и дистрибуции

Вывод команды show ip ospf database (LSDB):
``` ```

#### Настройка сети филиала в Луганской Народной Республике



Для настройки GRE-туннелей взята сеть 10.100.0.0/30, на R15 и R18 подняты интерфейсы
Loopback 0 с белыми IP-адресами (на их основе подняты туннели). Чтоб организовать связность
внутренних сетей офисов, интерфейсы туннелей (Tunnel 1) добавлены в процесс EIGRP, 
на R15 включена редистрибьюция из OSPF в EIGRP.
