
## IPSec over DmVPN.
### Задание: Настроить GRE поверх IPSec между офисами Москва и Санкт-Петербург, а также DMVPN поверх IPSec между Москвой, Чокурдахом и Лабытнанги.

#### Цель:
1.Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.  
2.Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.  
3.Все узлы в офисах в лабораторной работе должны иметь IP связность.  
4.План работы и изменения зафиксированы в документации.   

### Общая для всех маршрутизаторов часть
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
<img width="766" height="323" alt="изображение" src="https://github.com/user-attachments/assets/b29420d7-ea42-43b0-a019-b448bcf53dbf" />
<img width="633" height="752" alt="изображение" src="https://github.com/user-attachments/assets/c8bdd880-06a6-4023-9a71-cf1976496546" />
<img width="462" height="175" alt="изображение" src="https://github.com/user-attachments/assets/1e2ef134-460a-421e-b9db-a0409f58012e" />

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
<img width="574" height="833" alt="изображение" src="https://github.com/user-attachments/assets/a42e73ba-1244-4ba1-a15d-c46bca857596" />
<img width="490" height="103" alt="изображение" src="https://github.com/user-attachments/assets/85473d4d-5deb-4b8e-a7f9-02092c92ae41" />
<img width="499" height="101" alt="изображение" src="https://github.com/user-attachments/assets/cfd38157-cf29-423c-b482-3f08f8e86294" />
<img width="489" height="98" alt="изображение" src="https://github.com/user-attachments/assets/554f3c5b-9f35-4229-a566-2071ca28b3b9" />



