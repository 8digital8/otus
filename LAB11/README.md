
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
<img width="410" height="263" alt="изображение" src="https://github.com/user-attachments/assets/cc711b5a-7101-4e2f-8a64-3070cddc5c13" />

### Настроите BGP free core в офисе Санкт-Петербурга.

##### Пример конфигурации R18:
mpls label protocol ldp  
mpls ldp router-id Loopback0 force  
!  
interface Ethernet0/0  
 mpls ip                        
interface Ethernet0/1  
 mpls ip                        
   
##### Пример конфигурации R17:  
mpls label protocol ldp    
mpls ldp router-id Loopback0 force    
!    
interface Ethernet0/1    
 mpls ip      
 
##### Проверка:
<img width="512" height="170" alt="изображение" src="https://github.com/user-attachments/assets/a9cbeff9-0062-4d1a-b4f6-1671b311910d" />
<img width="561" height="500" alt="изображение" src="https://github.com/user-attachments/assets/5053f489-3269-47f2-86a0-5701554a333e" />
<img width="543" height="81" alt="изображение" src="https://github.com/user-attachments/assets/c4610126-d8b3-4e61-9951-be33cdacdf21" />
<img width="440" height="156" alt="изображение" src="https://github.com/user-attachments/assets/0e480d2d-f965-43db-a6f4-ec669a519804" />



