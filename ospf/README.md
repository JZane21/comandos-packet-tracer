# Comandos para levantar el servicio de Port-Channel

- En un router se debe configurar lo siguiente:

```pkt
- Router(config)#router opsf numero_grupo_del_ospf
- Router(config-router)#network red_grupo_ospf 0.0.0.255 area numero_area_ospf
```

### Default-Information Originate

Este commando es para compartir el last resort gateway. El last resort gateway es el router por el que enviaremos paquetes para una red determinada o para cualquier red si pusimo 0.0.0.0 0.0.0.0.

#### Comandos:

´´´
Router10(1) Router(config-if)#router ospf 1
Router10(1) Router(config-router)#network 192.168.90.0 0.0.0.255 area 0
´´´
´´´
Router11 Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1 //especificamos que traeremos cualquier red que conozca el router 10.10.10.1 directamente
Router11 Router(config)#router ospf 1
Router11 Router(config-if)#router ospf 1
Router11 Router(config-router)#network 192.168.100.0 0.0.0.255 area 0
Router11 Router(config-router)#default-information originate //con esto ya deberiamos tener comunicadas a las redes de los dos routers, es decir, las pc’s de ambos lados se deberian hacer ping
´´´
