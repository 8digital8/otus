
## Основные протоколы сети интернет.
### Задание: Настроить DHCP и синхронизацию времени в офисе Москва, а также NAT в офисах Москва, C.-Перетбруг и Чокурдах.

#### Цель:
1. Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
2. Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
3. Настроите статический NAT для R20.
4. Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
5. Настроите статический NAT(PAT) для офиса Чокурдах.
6. Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
7. Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
8. Все офисы в лабораторной работе должны иметь IP связность.

### Настроите NAT(PAT) на R14 и R15.   

##### Пример конфигурации R14,R15:
ip access-list standard NAT-MSK  
 permit 10.0.100.0 0.0.0.255  
 permit 10.0.101.0 0.0.0.255  
 permit 192.168.100.0 0.0.0.255  
 permit 192.168.101.0 0.0.0.255  
!  
interface Ethernet0/2  
 ip nat outside  
!  
interface ethernet 0/3  
 ip nat inside  
!
interface ethernet 0/0  
 ip nat inside
!  
interface ethernet 0/1  
 ip nat inside
! 
interface ethernet 1/0  
 ip nat inside
! 
ip nat inside source list NAT-MSK interface Ethernet0/2 overload

##### R15 (стык 172.16.0.28/30, адрес .29): то же, outside на e0/2, overload на interface Ethernet0/2.

### Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
К сожалению при планирование адресации на стыках применялись сети с маской /30 в связи этим нет возможности (не меняя адресацию) физически реализовать трансляцию в 5 IP адресов. Единственная реализация в рамках задания — PAT overload на оба интерфейсных адреса (R24-стык и R26-стык), что заодно закрывает требование балансировки по двум линкам.

##### Пример конфигурации R18:
ip access-list standard NAT-SPB  
 permit 10.0.102.0 0.0.0.255  
 permit 192.168.102.0 0.0.0.255  
 permit 192.168.103.0 0.0.0.255  
!  
interface Ethernet0/2  
 ip nat outside  
interface Ethernet0/3  
 ip nat outside  
!
interface Ethernet0/1    
ip nat inside  
!  
interface Ethernet0/0    
ip nat inside  
!
##### Чтобы PAT шёл через оба стыка (балансировка), используем route-map-вариант с двумя правилами:

route-map NAT-R24 permit 10  
 match ip address NAT-SPB  
 match interface Ethernet0/2  
route-map NAT-R26 permit 10  
 match ip address NAT-SPB   
 match interface Ethernet0/3  
!  
ip nat inside source route-map NAT-R24 interface Ethernet0/2 overload  
ip nat inside source route-map NAT-R26 interface Ethernet0/3 overload  

### Настроите статический NAT для R20.
##### Пример конфигурации R15:

ip nat inside source static tcp 10.0.100.20 22 interface Ethernet0/2 22  

### Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
##### Пример конфигурации R14:
ip nat inside source static tcp 10.0.100.19 22 interface Ethernet0/2 22  

### Настроите статический NAT(PAT) для офиса Чокурдах.
##### Пример конфигурации R28:
Аналогично адресации в AS2042 на стыках применялись сети с маской /30 в связи этим нет возможности (не меняя адресацию) физически реализовать статический NAT.

ip access-list standard NAT-CHOK  
 permit 10.0.102.0 0.0.0.255  
 permit 192.168.104.0 0.0.0.255  
 permit 192.168.105.0 0.0.0.255  
 permit 10.0.103.0 0.0.0.255  
!  
interface Ethernet0/0  
 ip nat outside                        
interface Ethernet0/1  
 ip nat outside                         
!    
interface Ethernet0/2   
 ip nat outside  
!  
ip nat inside source list NAT-CHOK interface Ethernet0/1 overload  

### Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
##### Пример конфигурации R12
ip dhcp pool wrk-1000  
 network 192.168.100.0 255.255.255.0  
 default-router 192.168.100.12  

##### R13 Аналогично только сеть 192.168.101.0 255.255.255.0

### Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
##### Пример конфигурации R12,R13
clock timezone SAKT 11   
ntp master 3  

###### Все прочие роутеры Москвы (R14, R15, R19, R20)
ntp server 10.0.100.12   
ntp server 10.0.100.13   
