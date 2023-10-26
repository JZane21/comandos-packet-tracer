# Ejemplo Ejercicio 3
![Ejercicio3](https://github.com/JZane21/comandos-packet-tracer/assets/71155282/dead9f9c-0933-432b-b90a-7a8e6549d54d)


## Router 0
```
conf t
ip nat inside source static 172.5.5.5 200.0.0.10 (ip del cliente) (ip del router al otro router)
interface f0/0.70
encapsulation dot1Q 70
ip address 172.5.5.1 255.255.255.0 (gateway del cliente y su mascara)
ip nat inside
no shutdown
exit

interface f0/0.80
encapsulation dot1Q 80
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface serial 0/3/0
ip address 200.0.0.1 255.255.255.0 (ip de la red entre routers y su mascara)
clock rate 64000
ip nat outside
no shutdown
exit
```

##Router 1
```
conf t
ip nat pool NAT-POOL 200.0.0.50 200.0.0.60 netmask 255.255.255.0
access-list 1 permit 10.10.10.0 0.0.0.255
ip nat inside source list 1 pool NAT-POOL

interface f0/0
ip address 10.10.10.1 255.255.255.0
ip nat inside
no shutdown
exit

interface serial 0/3/0
ip address 200.0.0.2 255.255.255.0
ip nat outside
no shutdown
exit
```

## Switch 0
```
conf t

(crear vlans)
vlan 70
vlan 80
exit

interface f0/2
switchport mode access
switchport access vlan 80
no shutdown
exit

(Crear portchannel 1 LACP)
interface range f0/23 - 24
channel-group 1 mode active
no shutdown
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 70,80
no shutdown
exit

interface f0/1
switchport mode trunk
switchport trunk allowed vlan 70,80
no shutdown
exit
```


## Switch 2
```
conf t

(crear vlans)
vlan 70
vlan 80
exit

interface f0/1
switchport mode access
switchport access vlan 70
no shutdown
exit

interface range f0/23 - 24
channel-group 1 mode active
no shutdown
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 70,80
no shutdown
exit
```
