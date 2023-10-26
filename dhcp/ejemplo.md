## Topología:  
![dhcpChallenge](https://github.com/JZane21/comandos-packet-tracer/assets/101936218/2c49f9db-dece-460d-8bbb-2c0525c29f72)  

## Comandos:  

### Router1:
#### Conectado al las pcs por medio de switch. Aqui tenemos 3 vlans, solamente le pasaremos dhcp a la vlan 30
```
   Router1   r1(config)#int fa 0/0.10
   Router1   r1(config-subif)#encapsulation dot1Q 10
   Router1   r1(config-subif)#ip add 192.168.10.1 255.255.255.0
   Router1   r1(config-subif)#no shutdown
   Router1   r1(config-subif)#int fa 0/0.20
   Router1   r1(config-subif)#encapsulation dot1Q 20
   Router1   r1(config-subif)#ip add 192.168.20.1 255.255.255.0
   Router1   r1(config-subif)#int fa 0/0.30
   Router1   r1(config-subif)#encapsulation dot1Q 30
   Router1   r1(config-subif)#ip add 192.168.30.1 255.255.255.0
   Router1   r1(config-subif)#int fa 0/0
   Router1   r1(config-if)#no shutdown
   Router1   r1(config-if)#int fa 0/1
   Router1   r1(config-if)#ip add 11.11.11.1 255.255.255.0 //conexion a router0
   Router1   r1(config-if)#no shutdown
```
### Router 0:
#### Conectado a servidor
```
   Router0   r2(config)#int fa 0/1
   Router0   r2(config-if)#ip add 11.11.11.2 255.255.255.0 //conexion a router1
   Router0   r2(config-if)#no shutdown
   Router0   r2(config-if)#int fa 0/0
   Router0   r2(config-if)#ip add 172.16.10.1 255.255.255.0 //conexion a servidor
   Router0   r2(config-if)#no shutdown
```
### Hacer que Router 1 conozca a servidor
```
   Router1   r1(config)#ip route 172.16.10.0 255.255.255.0 11.11.11.2
   Router1   r1(config)#do copy run start
```
### Hacer que Router 2 conozca las redes de las vlans
```
   Router0   r2(config)#ip route 192.168.10.0 255.255.255.0 11.11.11.1
   Router0   r2(config)#ip route 192.168.20.0 255.255.255.0 11.11.11.1
   Router0   r2(config)#ip route 192.168.30.0 255.255.255.0 11.11.11.1
```
### Router 1
#### Conectar servidor con vlan 30
Para que esto funcione es necesario hacer la configuracion en el servidor que se especificó en el Caso 2
```
   Router1   r1(config-if)#int fa 0/0.30
   Router1   r1(config-subif)#ip helper-address 172.16.10.10
```
