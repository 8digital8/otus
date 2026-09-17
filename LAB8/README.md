
## iBGP.
### Задание: BGP. Фильтрация
Цель:
Настроить фильтрацию в офисах Москва и С.-Петербург.

Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).     
Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).    
Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.    
Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.    
Все сети в лабораторной работе должны иметь IP связность.   

### Москва — запрет транзита (AS-path ^$), всё выученное по eBGP имеет непустой path и режется.   

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

### Киторн → Москва: только дефолт R22:

### Пример конфигурации R22:   

ip prefix-list ONLY-DEFAULT seq 5 permit 0.0.0.0/0  
!  
router bgp 101  
 neighbor 172.16.0.25 default-originate  
 neighbor 172.16.0.25 prefix-list ONLY-DEFAULT out  

### Ламас → Москва: дефолт + префикс СПб R21:
### Пример конфигурации R21:  

ip prefix-list DEF-SPB seq 5  permit 0.0.0.0/0   
ip prefix-list DEF-SPB seq 10 permit 10.0.102.0/24  
ip prefix-list DEF-SPB seq 15 permit 192.168.102.0/24  
ip prefix-list DEF-SPB seq 20 permit 192.168.103.0/24  
!  
router bgp 301  
 neighbor 172.16.0.29 default-originate  
 neighbor 172.16.0.29 prefix-list DEF-SPB out  

### Сквозная связность хостов (BGP↔IGP):  

### Пример конфигурации R14/R15:
router ospf 1  
 redistribute bgp 1001 subnets  
!  
### Пример конфигурации R18:  
router eigrp SPB  
 address-family ipv4 unicast autonomous-system 2042  
  topology base  
   redistribute bgp 2042 metric 1000000 100 255 1 1500  
  exit-af-topology  
 exit-address-family  
   


