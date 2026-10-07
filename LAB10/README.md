
## VPN. GRE. DmVPN.
### Задание: Настроить GRE между офисами Москва и Санкт-Петербург, а также DMVPN между Москвой, Чокурдахом и Лабытнанги.

#### Цель:
1. Настроите GRE между офисами Москва и С.-Петербург.  
2. Настроите DMVMN между Москва и Чокурдах, Лабытнанги.  
3. Все узлы в офисах в лабораторной работе должны иметь IP связность.  

### Настроите GRE между офисами Москва и С.-Петербург.   

##### Пример конфигурации R14:
interface Tunnel12  
 ip address 172.31.0.1 255.255.255.252  
 tunnel source Loopback0  
 tunnel destination 10.0.100.18  
 tunnel key 12  
 ip mtu 1400  
 ip tcp adjust-mss 1360  

##### Пример конфигурации R14:
interface Tunnel12  
 ip address 172.31.0.2 255.255.255.252  
 tunnel source Loopback0  
 tunnel destination 10.0.100.14  
 tunnel key 12  
 ip mtu 1400  
 ip tcp adjust-mss 1360  

##### Проверка:
<img width="701" height="616" alt="изображение" src="https://github.com/user-attachments/assets/131bbb3e-cc47-4e68-8b6f-06d9acc06679" />  

### Настроите DMVMN между Москва и Чокурдах, Лабытнанги. 
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

##### Пример конфигурации R27:
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

##### Пример конфигурации R28:
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

