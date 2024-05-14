# IPSec over DmVPN


----

## Задание:

* Настроить GRE поверх IPSec между офисами Москва и С.-Петербург.
* Настроить DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.
* Все узлы в офисах в лабораторной работе должны иметь IP связность.
* План работы и изменения зафиксированы в документации.



### Схема реализации (EVE-NG)
\
![BigNet.png](img%2FBigNet.png)

Настройки всех сетевых устройств приведены по [ссылке](configs%2Freadme.md).


### Настройка центра сертификаций CA на R15
```
ip domain-name otus.ru
ip http server
crypto key generate rsa general-keys label CA exportable modulus 2048
crypto pki server CA
database level complete
no shut
```


Клиенты (VPN peers)
R18, R15:

```
crypto key generate rsa label VPN modulus 2048
crypto pki trustpoint VPN
enrollment url http://10.100.0.1:80
revocation-check none
(config)#crypto pki authenticate VPN
(config)#crypto pki enroll VPN
```

R27, R28:
```
crypto key generate rsa label VPN modulus 2048
crypto pki trustpoint VPN
enrollment url http://10.200.0.1:80
revocation-check none
(config)#crypto pki authenticate VPN
(config)#crypto pki enroll VPN
```

На CA подпишем сертификаты
```
crypto pki server CA grant [1-999|all]
R15#show crypto pki server CA certificates
Serial Issued date              Expire date               Subject Name
1       22:01:18 MSK May 14 2024 22:01:18 MSK May 14 2027  cn=CA
2       22:05:06 MSK May 14 2024 22:05:06 MSK May 14 2025  hostname=R15.otus.ru
3       22:08:53 MSK May 14 2024 22:08:53 MSK May 14 2025  hostname=R18
4       22:19:38 MSK May 14 2024 22:19:38 MSK May 14 2025  hostname=R27
5       22:56:50 MSK May 14 2024 22:56:50 MSK May 14 2025  hostname=R28

```


### Настройка GRE поверх IPSec между офисами Москва и С.-Петербург

Первая фаза IKEv2 на R15:
```
crypto ikev2 proposal PROSPB
encryption aes-cbc-128
integrity md5
group 2
crypto ikev2 policy IKEV2
proposal PROSPB
crypto ikev2 profile PROFILE1
match address local interface Loopback0
match identity remote address 0.0.0.0
authentication remote rsa-sig
authentication local rsa-sig
pki trustpoint VPN
```

Вторая фаза на R15:

```
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
mode tunnel
crypto ipsec profile IPSEC_PROF
set transform-set IPSEC_TS
set ikev2-profile PROFILE1
```


Первая и вторая фазы на R18:

```
crypto ikev2 proposal PROMSK
encryption aes-cbc-128
integrity md5
group 2
crypto ikev2 policy IKEV2
proposal PROMSK
crypto ikev2 profile PROFILE1
match address local interface Loopback0
match identity remote address 0.0.0.0
authentication remote rsa-sig
authentication local rsa-sig
pki trustpoint VPN
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
mode tunnel
crypto ipsec profile IPSEC_PROF
set transform-set IPSEC_TS
set ikev2-profile PROFILE1
```

На туннельных интерфейсах Tunnel1 R15, R18:
```
tunnel protection ipsec profile IPSEC_PROF
```

Проверка на R15:

```
R15#ping 10.0.9.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.9.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 9/10/13 ms

R15#show crypto ikev2 sa
 IPv4 Crypto IKEv2  SA

Tunnel-id Local                 Remote                fvrf/ivrf            Status
1         100.1.0.10/500        20.42.0.17/500        none/none            IN-NEG
      Encr: Unknown - 0, PRF: Unknown - 0, Hash: None, DH Grp:0, Auth sign: Unknown - 0, Auth verify: Unknown - 0
      Life/Active Time: 86400/0 sec

 IPv6 Crypto IKEv2  SA

R15#show crypto ipsec sa

interface: Tunnel1
    Crypto map tag: Tunnel1-head-0, local addr 100.1.0.10

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (100.1.0.10/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (20.42.0.17/255.255.255.255/47/0)
   current_peer 20.42.0.17 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 90, #pkts encrypt: 90, #pkts digest: 90
    #pkts decaps: 91, #pkts decrypt: 91, #pkts verify: 91
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 100.1.0.10, remote crypto endpt.: 20.42.0.17
     plaintext mtu 1438, path mtu 1500, ip mtu 1500, ip mtu idb Ethernet0/2
     current outbound spi: 0xC93A0BAF(3376024495)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x8D9B8F11(2375782161)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 7, flow_id: SW:7, sibling_flags 80000040, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4157176/3226)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xC93A0BAF(3376024495)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 8, flow_id: SW:8, sibling_flags 80000040, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4157176/3226)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

```


### Настройка DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.
   

Создадим на R15 новый профиль IPSEC:
```
crypto ipsec transform-set DMVPN esp-aes esp-md5-hmac
mode transport
crypto ipsec profile DMVPN_PROF
set transform-set DMVPN
set ikev2-profile PROFILE1
```

Настройка на R27 и R28 для DMVPN:
```
crypto ikev2 proposal PROMSK
encryption aes-cbc-128
integrity md5
group 2
crypto ikev2 policy IKEV2
proposal PROMSK
crypto ikev2 profile PROFILE1
match address local interface Ethernet0/0
match identity remote address 0.0.0.0
authentication remote rsa-sig
authentication local rsa-sig
pki trustpoint VPN
crypto ipsec transform-set DMVPN esp-aes esp-md5-hmac
mode transport
crypto ipsec profile DMVPN_PROF
set transform-set DMVPN
set ikev2-profile PROFILE1
```


Проверка на R27
```
R27#ping 10.0.3.129
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.3.129, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 9/10/11 ms
R27#show crypto ikev2 sa
 IPv4 Crypto IKEv2  SA

Tunnel-id Local                 Remote                fvrf/ivrf            Status
1         52.0.0.2/500          100.1.0.10/500        none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/375 sec

 IPv6 Crypto IKEv2  SA

R27#show crypto ipsec sa

interface: Tunnel2
    Crypto map tag: Tunnel2-head-0, local addr 52.0.0.2

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (52.0.0.2/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (100.1.0.10/255.255.255.255/47/0)
   current_peer 100.1.0.10 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 65, #pkts encrypt: 65, #pkts digest: 65
    #pkts decaps: 68, #pkts decrypt: 68, #pkts verify: 68
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 52.0.0.2, remote crypto endpt.: 100.1.0.10
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb (none)
     current outbound spi: 0xE8E717A3(3907458979)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0xE2CCA061(3805061217)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 2, flow_id: SW:2, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4174347/3217)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xE8E717A3(3907458979)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 1, flow_id: SW:1, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4174348/3217)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

```

Все фазы на R15:

```
R15#show crypto ikev2 sa
 IPv4 Crypto IKEv2  SA

Tunnel-id Local                 Remote                fvrf/ivrf            Status
3         100.1.0.10/500        52.0.0.10/500         none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/378 sec

Tunnel-id Local                 Remote                fvrf/ivrf            Status
4         100.1.0.10/500        20.42.0.17/500        none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/366 sec

Tunnel-id Local                 Remote                fvrf/ivrf            Status
2         100.1.0.10/500        52.0.0.2/500          none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/430 sec

 IPv6 Crypto IKEv2  SA

R15#show crypto ipsec sa

interface: Tunnel1
    Crypto map tag: Tunnel1-head-0, local addr 100.1.0.10

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (100.1.0.10/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (20.42.0.17/255.255.255.255/47/0)
   current_peer 20.42.0.17 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 90, #pkts encrypt: 90, #pkts digest: 90
    #pkts decaps: 91, #pkts decrypt: 91, #pkts verify: 91
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 100.1.0.10, remote crypto endpt.: 20.42.0.17
     plaintext mtu 1438, path mtu 1500, ip mtu 1500, ip mtu idb Ethernet0/2
     current outbound spi: 0xC93A0BAF(3376024495)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x8D9B8F11(2375782161)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 7, flow_id: SW:7, sibling_flags 80000040, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4157176/3226)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xC93A0BAF(3376024495)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 8, flow_id: SW:8, sibling_flags 80000040, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4157176/3226)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

interface: Tunnel2
    Crypto map tag: Tunnel2-head-0, local addr 100.1.0.10

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (100.1.0.10/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (52.0.0.10/255.255.255.255/47/0)
   current_peer 52.0.0.10 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 51, #pkts encrypt: 51, #pkts digest: 51
    #pkts decaps: 50, #pkts decrypt: 50, #pkts verify: 50
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 100.1.0.10, remote crypto endpt.: 52.0.0.10
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb (none)
     current outbound spi: 0x9D20F743(2636183363)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x13DBD3A6(333173670)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 3, flow_id: SW:3, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4272506/3215)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0x9D20F743(2636183363)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 4, flow_id: SW:4, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4272505/3215)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (100.1.0.10/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (52.0.0.2/255.255.255.255/47/0)
   current_peer 52.0.0.2 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 74, #pkts encrypt: 74, #pkts digest: 74
    #pkts decaps: 71, #pkts decrypt: 71, #pkts verify: 71
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 100.1.0.10, remote crypto endpt.: 52.0.0.2
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb (none)
     current outbound spi: 0xE2CCA061(3805061217)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0xE8E717A3(3907458979)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 1, flow_id: SW:1, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4325189/3162)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xE2CCA061(3805061217)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 2, flow_id: SW:2, sibling_flags 80000000, crypto map: Tunnel2-head-0
        sa timing: remaining key lifetime (k/sec): (4325188/3162)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

```