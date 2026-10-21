
## IPSec over DmVPN.
### Задание: Настроить GRE поверх IPSec между офисами Москва и Санкт-Петербург, а также DMVPN поверх IPSec между Москвой, Чокурдахом и Лабытнанги.

#### Цель:
1.Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.  
2.Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.  
3.Все узлы в офисах в лабораторной работе должны иметь IP связность.  
4.План работы и изменения зафиксированы в документации.   

### Общая для всех маршрутизатров часть
crypto ikev2 proposal PROP-1  
 encryption aes-cbc-256  
 integrity sha256  
 group 14  
!  
crypto ikev2 policy POL-1  
 proposal PROP-1  
!  
crypto ikev2 keyring KR  
 peer ANY  
  address 0.0.0.0 0.0.0.0  
  pre-shared-key CISCO-DMVPN-KEY  
!  
crypto ikev2 profile IKEV2-PROF  
 match identity remote address 0.0.0.0  
 authentication local pre-share  
 authentication remote pre-share  
 keyring local KR  
!  
crypto ipsec transform-set TS esp-aes 256 esp-sha256-hmac  
 mode transport  
!  
crypto ipsec profile IPSEC-PROF  
 set transform-set TS  
 set ikev2-profile IKEV2-PROF  

### Настроите GRE поверх IPSec между офисами Москва и С.-Петербург..   

##### Пример конфигурации R14:  
interface Tunnel12  
 ip address 172.31.0.1 255.255.255.252  
 tunnel source Loopback0  
 tunnel destination 10.0.100.18  
 tunnel key 12  
 ip mtu 1400  
 ip tcp adjust-mss 1360  
 tunnel protection ipsec profile IPSEC-PROF  
 
##### Пример конфигурации R18:  
interface Tunnel12  
 ip address 172.31.0.2 255.255.255.252  
 tunnel source Loopback0  
 tunnel destination 10.0.100.14  
 tunnel key 12  
 ip mtu 1400  
 ip tcp adjust-mss 1360  
 tunnel protection ipsec profile IPSEC-PROF  
##### Проверка

### Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.ь

##### Пример конфигурации R14:
interface Tunnel1  
 ip address 172.31.1.1 255.255.255.0  
 tunnel source Loopback0  
 tunnel mode gre multipoint  
 tunnel key 1  
 ip nhrp network-id 1  
 ip nhrp map multicast dynamic  
 ip nhrp redirect  
 ip mtu 1400  
 ip tcp adjust-mss 1360  
 tunnel protection ipsec profile IPSEC-PROF  

##### Пример конфигурации R28:
interface Tunnel1  
 ip address 172.31.1.28 255.255.255.0  
 tunnel source Loopback0  
 tunnel mode gre multipoint  
 tunnel key 1  
 ip nhrp network-id 1  
 ip nhrp nhs 172.31.1.1 nbma 10.0.100.14 multicast  
 ip nhrp shortcut  
 ip mtu 1400  
 ip tcp adjust-mss 1360  
 tunnel protection ipsec profile IPSEC-PROF  

##### Пример конфигурации R27:

interface Tunnel1  
 ip address 172.31.1.27 255.255.255.0  
 tunnel source Loopback0  
 tunnel mode gre multipoint  
 tunnel key 1  
 ip nhrp network-id 1  
 ip nhrp nhs 172.31.1.1 nbma 10.0.100.14 multicast  
 ip nhrp shortcut  
 ip mtu 1400  
 ip tcp adjust-mss 1360  
 tunnel protection ipsec profile IPSEC-PROF 

##### Проверка



