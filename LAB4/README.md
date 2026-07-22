# IS-IS
## Задание:
1. Настроить протокол IS-IS для IPv4 и IPv6 в ISP AS 520 Триада.

R23 и R25 находятся в зоне 2222.
R24 находится в зоне 24.
R26 находится в зоне 26

## Пример кнфигурации:
R25
router isis   
 net 49.2222.0000.0000.0025.00      
 metric-style wide      

 interface Loopback0    
 description routers_managment    
 ip address 10.0.100.25 255.255.255.255    
 ip router isis    
   
interface Ethernet0/0    
 description to-r23    
 ip address 172.16.0.46 255.255.255.252    
 ip router isis
