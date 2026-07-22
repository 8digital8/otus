
## EIGRP
## Задание:
1. Настроить EIGRP named-mode для IPv4 в AS2042 офисе С.-Петербург.

В офисе С.-Петербург настроить EIGRP.        
R32 получает только маршрут по умолчанию.        
R16-17 анонсируют только суммарные префиксы.        
Использовать EIGRP named-mode для настройки сети.        

R16-17 анонсируют только суммарные префиксы. Данный пункт не полностью выполнен, так как адресация не позволяют сделать один суммированный маршрут. Суммарный префикс только один, остальные прекфиксы передаются как есть через network.

## Пример кнфигурации:

router eigrp SPB            
 !        
 address-family ipv4 unicast autonomous-system 2042        
  !        
  af-interface Ethernet0/1        
   summary-address 192.168.102.0 255.255.254.0        
  exit-af-interface        
  !        
  topology base        
  exit-af-topology        
  network 10.0.100.17 0.0.0.0        
  network 172.16.0.68 0.0.0.3        
  network 192.168.102.0 0.0.1.255        
  eigrp router-id 10.0.100.17        
 exit-address-family        
        
