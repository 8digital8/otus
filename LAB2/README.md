#  PBR

##  Задание:
1.Настроить политику маршрутизации для сетей офиса.    
2.Распределить трафик между двумя линками с провайдером.    
3.Настроить отслеживание линка через технологию IP SLA.(только для IPv4)    
4.Настройть для офиса Лабытнанги маршрут по-умолчанию.      
5.План работы и изменения зафиксированы в документации.

### 1.Настроите политику маршрутизации для сетей офиса.
Маршрутизаторый r12 и r13 перенаправляют HTTP/s и FTP траффик по разным маршрутизаторам.     
Пример конфигурации:   
access-list 100 permit tcp any any eq 80  
access-list 100 permit tcp any any eq 443  
access-list 101 permit tcp any any eq 20  
access-list 101 permit tcp any any eq 21  

route-map PBR_SERVICES permit 10  
 match ip address 100  
 set ip next-hop 172.16.0.6  

route-map PBR_SERVICES permit 20  
 match ip address 101  
 set ip next-hop 172.16.0.10       

interface Ethernet0/0.1000  
 ip policy route-map PBR_SERVICES  
 
interface Ethernet0/0.1001  
 ip policy route-map PBR_SERVICES   

### 2.Распределите трафик между двумя линками с провайдером.
Маршрутизатор R18 распределяет траффик между двуми мрашрутизаторами AS520 по сети источника пакета.  
Пример конфигурации:  
access-list 100 permit ip 192.168.102.0 0.0.0.255 any  
access-list 101 permit ip 192.168.103.0 0.0.0.255 any  
route-map PBR permit 10 
 match ip address 100  
 set ip next-hop 172.16.0.62  
route-map PBR permit 20  
 match ip address 101   
 set ip next-hop 172.16.0.66   
interface Ethernet0/1  
 ip policy route-map PBR  
 interface Ethernet0/0  
 ip policy route-map PBR  
 
### 3.Настроить отслеживание линка через технологию IP SLA.(только для IPv4)
Коммутаторы SW2, SW3, SW4, SW5, SW9 и SW10 используют ip sla для переключения маршрута по умлочанию.  
Пример конфигурации:  
ip route 0.0.0.0 0.0.0.0 10.0.101.13 track 1  
ip route 0.0.0.0 0.0.0.0 10.0.101.13  
ip route 0.0.0.0 0.0.0.0 10.0.101.12  
ip sla 1  
 icmp-echo 10.0.101.13 source-interface Vlan101  
 frequency 5  
ip sla schedule 1 life forever start-time now  

### 4. Настройть для офиса Лабытнанги маршрут по-умолчанию.
ip route 0.0.0.0 0.0.0.0 172.16.0.81
