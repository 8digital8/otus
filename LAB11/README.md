
## Базовый сервис MPLS.
### Задание: Настроить BGP free core в офисах Москвы и Санкт-Петербурга.

#### Цель:
1. Настроите BGP free core в офисе Москвы.
2. Настроите BGP free core в офисе Санкт-Петербурга.
3. План работы и изменения зафиксированы в документации.  

### Настроите BGP free core в офисе Москвы.   

##### Пример конфигурации R14:  
mpls label protocol ldp  
mpls ldp router-id Loopback0 force  
!  
interface Ethernet0/0  
 mpls ip                    
interface Ethernet0/1  
 mpls ip                        
interface Ethernet0/3  
 mpls ip                        
interface Ethernet1/0  
 mpls ip  
 
##### Пример конфигурации R15:  
mpls label protocol ldp  
mpls ldp router-id Loopback0 force  
!  
interface Ethernet0/0  
 mpls ip                        
interface Ethernet0/1  
 mpls ip                       
interface Ethernet0/3  
 mpls ip                       
interface Ethernet1/0  
 mpls ip  
    
##### Пример конфигурации R12:
mpls label protocol ldp  
mpls ldp router-id Loopback0 force  
!
interface Ethernet0/2  
 mpls ip                     
interface Ethernet0/3  
 mpls ip                     
interface Ethernet0/0  
 mpls ip       

##### Проверка R14:
<img width="523" height="560" alt="изображение" src="https://github.com/user-attachments/assets/7b99ca7d-72ca-49ec-946b-010bab3cbc27" />
<img width="565" height="284" alt="изображение" src="https://github.com/user-attachments/assets/4b6c3c6f-0a37-4fa1-85dc-8bae3da54306" />
<img width="545" height="84" alt="изображение" src="https://github.com/user-attachments/assets/0f44adfa-d1c1-4b70-a0db-d3eefbf0edfd" />





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
##### Проверка:
<img width="565" height="239" alt="изображение" src="https://github.com/user-attachments/assets/774dfa9d-edc3-4c16-883e-343111591dd0" />  
<img width="393" height="138" alt="изображение" src="https://github.com/user-attachments/assets/ff4a6fa2-fca9-4ec5-8107-29843af43613" />  
<img width="547" height="216" alt="изображение" src="https://github.com/user-attachments/assets/74fac9ca-2a12-4f13-938c-9e7f3e7de357" />  
<img width="501" height="174" alt="изображение" src="https://github.com/user-attachments/assets/ee0cd175-5e25-4a46-8e86-c4d1f30396e8" />




