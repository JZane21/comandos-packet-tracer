# NAT dinámico y estático, LACP

Recordemos la práctica 3:

![image-20231025135511929](C:\Users\ANDER\AppData\Roaming\Typora\typora-user-images\image-20231025135511929.png)

Estos son los pasos para configurar el NATEO y el protocolo LACP:

1. Colocar las IP's correspondientes a los servidores (así también su máscara de red y su gateway).

   TRUCAZO: Copiar comandos y pegar directamente en la consola de los dispositivos de Packet Tracer. 

2. Configurar los Routers:

```cmd
## En el router 0
## La configuración de subinterfaces

## Para la VLAN 70
int f0/0.70
encapsulation dot1Q 70
ip address 172.5.5.1 255.255.255.0
ip nat inside
no shutdown

## Para la VLAN 80
int f0/0.80
encapsulation dot1Q 80
ip address 192.168.10.1 255.255.255.0
no shutdown

## Finalmente
int f0/0
no shutdown
```

```cmd
## En el router 1
## La configuración de interfaces
int f0/0
ip address 10.10.10.1 255.255.255.0
no shutdown
```

Recuerda hacer ping para hacer la prueba del correcto funcionamiento.

3. Configurar los switches:

```cmd
# En el Switch 0

# Crear VLAN's
vlan 70
vlan 80

# Colocar en modo acceso los puertos:
int f0/2
switchport mode access
switchport access vlan 80

int f0/1
switchport mode trunk
switchport trunk allowed vlan 70,80
```

```cmd
# En el Switch 2

#Crear VLAN 70
vlan 70

# Colocar en modo acceso los puertos:
int f0/1
switchport mode access
switchport access vlan 70
```

4. Configurando el LACP en los switches

```cmd
## Ahora configurando el LACP en los switches

## Hacer el siguiente procedimiento con los dos switches

## Encender ambos puertos
int r f0/23 - 24
no shutdown

## Configurar los puertos en LACP
int r f0/23 - 24
channel-group 1 mode active

## Crear la interfaz del port-channel 
int port-channel1
switchport mode trunk
switchport trunk allowed vlan 70,80

## Para hacer troubleshooting
show interfaces status
show interfaces port-channel 1
show etherchannel summary
```

En el Switch 0:

En el Switch 2:

Eso indica un buen funcionamiento del LACP en nuestra red:

5. Nuevamente en los routers para el NATEO:

```cmd
# En el router 0 NATEO Estático
int s0/3/0
ip address 200.0.0.1 255.255.255.0
clock rate 64000 ## No olvidar, solo en un router
no shutdown

###############################################
# El Nateo hacia el server, en el router 0
ip nat inside source static 172.5.5.5 200.0.0.10

ip route 10.10.10.0 255.255.255.0 200.0.0.2 
## Por dentro (privado)
int f0/0
ip nat inside

## Público, por fuera
int s0/3/0
ip nat outside
###############################################

## Para hacer troubleshooting
show ip nat translations
show ip nat statistics
```

```cmd
# En el router 1 NATEO Dinámico
int s0/3/0
ip address 200.0.0.2 255.255.255.0
no shutdown

## Las direcciones para el NATEO
ip nat pool NAT-POOL 200.0.0.50 200.0.0.60 netmask 255.255.255.0

### Ahora si se viene la lista de acceso --- rango del 1 al 100
access-list 1 permit 10.10.10.0 0.0.0.255
ip nat inside source list 1 pool NAT-POOL

ip route 172.5.5.0 255.255.255.0 200.0.0.1 
ip route 192.168.10.0 255.255.255.0 200.0.0.1 

int f0/0
ip nat inside

int s0/3/0
ip nat outside
```

