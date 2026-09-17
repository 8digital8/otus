
## iBGP.
### Задание: BGP. Фильтрация
Цель:
Настроить фильтрацию в офисах Москва и С.-Петербург.

Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).     
Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).    
Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.    
Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.    
Все сети в лабораторной работе должны иметь IP связность.   

Москва — запрет транзита (AS-path ^$), всё выученное по eBGP имеет непустой path и режется.   

### Пример конфигурации R14,R15:
ip as-path access-list 10 permit ^$      
!    
router bgp 1001      
 neighbor 172.16.0.26 filter-list 10 out (для R15 172.16.0.28)       
!        

### Проверка R14 анонсирует только свои сети: 
<img width="570" height="347" alt="изображение" src="https://github.com/user-attachments/assets/f79808d9-be53-468a-8489-a6fe00cd19dd" />

### С.-Петербург — запрет транзита (prefix-list)
Тот же смысл, инструментом prefix-list: к R24 и R26 наружу пускаем только собственные сети СПб, остальное режет implicit deny.

### Пример конфигурации R18:        

ip prefix-list SPB-OWN seq 5 permit 192.168.102.0/23  
ip prefix-list SPB-OWN seq 10 permit 10.0.102.0/24  
ip prefix-list SPB-OWN seq 20 permit 10.0.100.16/32  
ip prefix-list SPB-OWN seq 25 permit 10.0.100.17/32  
ip prefix-list SPB-OWN seq 30 permit 10.0.100.18/32  
ip prefix-list SPB-OWN seq 35 permit 10.0.100.32/32  

router bgp 2042  
 neighbor 172.16.0.62 prefix-list SPB-OWN out  
 neighbor 172.16.0.66 prefix-list SPB-OWN out  

### Проверка R18 анонсирует только свои сети:
<img width="566" height="243" alt="изображение" src="https://github.com/user-attachments/assets/b15d9298-f168-49fc-9979-0041f5a6408a" />






### Проверка связности:

msk-as1001-r12#ping 192.168.102.17        
Type escape sequence to abort.        
Sending 5, 100-byte ICMP Echos to 192.168.102.17, timeout is 2 seconds:        
!!!!!        
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms        
msk-as1001-r12#tracer        
msk-as1001-r12#traceroute 192.168.102.17        
Type escape sequence to abort.        
Tracing the route to 192.168.102.17        
VRF info: (vrf in name/id, vrf out name/id)        
  1 172.16.0.6 0 msec        
    172.16.0.10 1 msec        
    172.16.0.6 1 msec        
  2 172.16.0.30 0 msec        
    172.16.0.94 2 msec        
    172.16.0.30 1 msec        
  3 172.16.0.30 1 msec        
    172.16.0.42 2 msec        
    172.16.0.30 4 msec        
  4 172.16.0.61 5 msec        
    172.16.0.42 5 msec        
    172.16.0.61 8 msec        
  5 172.16.0.61 8 msec        
    172.16.0.69 6 msec        
    172.16.0.61 2 msec        
msk-as1001-r12#        
        


        


