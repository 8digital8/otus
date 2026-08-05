## iBGP.
### Задание:
Настроить iBGP в офисе Москва и в сети провайдера Триада для обеспечения полной IP-связности всех сетей.

    Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.
    Настроите iBGP в провайдере Триада, с использованием RR.
    Настройте офиса Москва так, чтобы приоритетным провайдером стал Ламас.
    Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
    Все сети в лабораторной работе должны иметь IP связность.

Анонсируются только loopback сети, дистрибуция маршрутов не делается.

### Пример конфигурации R14:

router bgp 1001        
 bgp router-id 10.0.100.14        
 bgp log-neighbor-changes        
 network 10.0.100.14 mask 255.255.255.255        
 neighbor 10.0.100.15 remote-as 1001        
 neighbor 10.0.100.15 update-source Loopback0        
 neighbor 10.0.100.15 next-hop-self        
 neighbor 172.16.0.26 remote-as 101        

### Проверка связности:

msk-as1001-r14#traceroute 10.0.100.18 source 10.0.100.14        
Type escape sequence to abort.        
Tracing the route to 10.0.100.18        
VRF info: (vrf in name/id, vrf out name/id)        
  1 172.16.0.94 0 msec 1 msec 0 msec        
  2 172.16.0.30 1 msec 1 msec 2 msec        
  3 172.16.0.42 2 msec 2 msec 2 msec        
  4 172.16.0.61 2 msec 1 msec *        

msk-as1001-r14#ping 10.0.100.18 source 10.0.100.14        
Type escape sequence to abort.        
Sending 5, 100-byte ICMP Echos to 10.0.100.18, timeout is 2 seconds:        
Packet sent with a source address of 10.0.100.14        
!!!!!        
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms        


        


