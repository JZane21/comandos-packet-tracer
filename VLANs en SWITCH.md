## Acceder al modo de configuración global
    
    enable
    configure terminal

## Crear una VLAN (Ejemplo: VLAN 10)
  ```shell
  vlan 10
  ```

## Asignar un nombre a la VLAN
  ```
  name voice
  ```

## Salir del modo de configuración de VLAN
  ```
  exit
  ```

## Acceder a la configuración del puerto (Ejemplo: FastEthernet0/1)
  ```
  interface FastEthernet0/1
  ```

## Asignar un puerto a una VLAN
  ```
  switchport mode access
  switchport access vlan 10
  ```

## Verificar la configuración de VLAN
  ```
  show vlan
  ```
