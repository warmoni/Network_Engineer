# Маршрутизация на основе политик (PBR)

----

## Задание:

* Настроите политику маршрутизации для сетей офиса.
* Распределите трафик между двумя линками с провайдером.
* Настроите отслеживание линка через технологию IP SLA.(только для IPv4).
* Настройте для офиса Лабытнанги маршрут по-умолчанию.
* План работы и изменения зафиксированы в документации.



### Схема реализации (EVE-NG)
\
![scheme.png](img%2FPBR.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).

### Настройка PBR и IP SLA

Распределение трафика в офисе Чокурдах между провайдерами произведено следующим 
образом: чётные адреса из сети 10.0.29.0/24 имеют next-hop 52.0.0.5, нечётные - 52.0.0.9.
Для этого использовлась карта маршрутов Balance_29. Чтобы мониторить состояние линков
между офисами в Триаде и Чокурдах использована технология IP SLA. Настройки приведены ниже:

```
track 1 ip sla 1 reachability
 delay down 10 up 10
!
track 2 ip sla 2 reachability
 delay down 10 up 10
!
ip route 0.0.0.0 0.0.0.0 52.0.0.9 50 track 1
ip route 0.0.0.0 0.0.0.0 52.0.0.5 100
!
ip access-list standard Even
 permit 10.0.29.0 0.0.0.254
 deny   any
!
ip sla 1
 icmp-echo 52.0.0.9 source-ip 52.0.0.10
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-echo 52.0.0.5 source-ip 52.0.0.6
 threshold 2000
 timeout 2000
 frequency 4
ip sla schedule 2 life forever start-time now
!
route-map Balance_29 permit 10
 match ip address Even
 set ip next-hop verify-availability 52.0.0.5 10 track 2
```

### Проверка PBR и IP SLA.

VPC30 - ip address 10.0.29.2
VPC31 - ip address 10.0.29.3

Трассировка на адрес 8.8.8.8

![trace30.png](img%2Ftrace30.png)


![trace31.png](img%2Ftrace31.png)

Трассировка на тот же адрес после падения линка между R28 и R26:

![trace30_2.png](img%2Ftrace30_2.png)


![trace31_2.png](img%2Ftrace31_2.png)

Логи на роутере R28:

```
*Jan  9 19:14:11.631: %TRACK-6-STATE: 1 ip sla 1 reachability Up -> Down
*Jan  9 19:15:46.681: %TRACK-6-STATE: 1 ip sla 1 reachability Down -> Up
*Jan  9 19:34:24.697: %SYS-5-CONFIG_I: Configured from console by console
*Jan  9 19:49:27.710: %TRACK-6-STATE: 2 ip sla 2 reachability Up -> Down
*Jan  9 19:52:52.810: %TRACK-6-STATE: 2 ip sla 2 reachability Down -> Up
```

Статистика IP SLA на R28:

```
R28#sh ip sla statistics
IPSLAs Latest Operation Statistics

IPSLA operation id: 1
        Latest RTT: 1 milliseconds
Latest operation start time: 22:56:01 MSK Tue Jan 9 2024
Latest operation return code: OK
Number of successes: 347
Number of failures: 0
Operation time to live: Forever

IPSLA operation id: 2
        Latest RTT: 1 milliseconds
Latest operation start time: 22:56:00 MSK Tue Jan 9 2024
Latest operation return code: OK
Number of successes: 541
Number of failures: 52
Operation time to live: Forever
```

### Настройка маршрута по-умолчанию в офисе Лабытнанги

```
ip route 0.0.0.0 0.0.0.0 52.0.0.1
```

