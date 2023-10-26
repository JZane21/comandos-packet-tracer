### EIGRP 7w7
----> El truco es que todos esten en la misma eigrp 7w7
#### TOPOLOGIA 
![image](https://github.com/JZane21/comandos-packet-tracer/assets/40927763/830d34f9-a82c-4e8e-bd27-291354883114)


#### Router 4

Añadir ip's
```
interface FastEthernet0/0
 ip address 192.168.50.1 255.255.255.0


interface Serial0/3/0
 ip address 10.10.10.2 255.255.255.0
 clock rate 64000
```

Poner el eigrp ---> todos los routers en las misma eigrp y añadir las redes que se van a enviar y conectar, y sus wilcard 7-7

```
router eigrp 1
network 192.168.50.0 0.0.0.255
network 10.10.10.0 0.0.0.255
```

#### Router 6

Añadir ip's
```
interface Serial0/3/0
ip address 10.10.10.1 255.255.255.0


interface Serial0/3/1
ip address 20.20.20.1 255.255.255.0
clock rate 64000
```

Poner el eigrp ---> todos los routers en las misma eigrp y añadir las redes que se van a enviar y sus wilcard 7-7
```
router eigrp 1
network 10.10.10.0 0.0.0.255
network 20.20.20.0 0.0.0.255
```


#### Router 5
Añadir ip's

```
interface FastEthernet0/0
ip address 172.16.10.1 255.255.255.0

interface Serial0/3/0
ip address 20.20.20.2 255.255.255.0
```
Poner el eigrp ---> todos los routers en las misma eigrp y añadir las redes que se van a enviar y sus wilcard 7-7

```
router eigrp 1
network 20.20.20.0 0.0.0.255
network 172.16.10.0 0.0.0.255
```

#### NOTAS  
Si queremos redistribuir ip's estaticas por todo el eigrp poner esto en el router que tiene guardada la ip estatica:
```pk
Vivian2CORE(config)#router eigrp 1
Vivian2CORE(config-router)#red static
```
Si nos conectamos a usuarios finales por medio de alguna interfaz, no es necesario mandarle ciertos paquetes:
```
Vivian0(config)#router eigrp 1
Vivian0(config-router)#passive-interface fa 0/0
```
