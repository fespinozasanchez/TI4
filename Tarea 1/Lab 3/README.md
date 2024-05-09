# Lab 4.5.2 configuración Básica
![](https://beta.appflowy.cloud/api/file_storage/5797f5ae-32d4-4fc8-b351-ff49f1e56fb9/blob/13944919312481542585.png)

### Parte 1: Armar la red y configurar los ajustes básicos de los dispositivos
En la parte 1, establecerá la topología de la red y configurará los parámetros básicos en los equipos host y los switches.
#### Paso 1: Realizar el cableado de red como se muestra en la topología
Conecte los dispositivos como se muestra en la topología y realizar el cableado necesario.

#### Paso 2: configurar los parámetros básicos para el router.
* a. Acceda al router mediante el puerto de consola y habilite el modo EXEC con privilegios.
* b. Ingrese al modo de configuración.
* c. Asigne un nombre de dispositivo al router.
* d. Inhabilite la búsqueda DNS para evitar que el router intente traducir los comandos mal introducidos como si fueran nombres de host.
* e. Asigne class como la contraseña cifrada del modo EXEC privilegiado.
* f. Asigne cisco como la contraseña de la consola y habilite el inicio de sesión.
* g. Asigne cisco como la contraseña de vty y habilite el inicio de sesión.
* h. Cifre las contraseñas de texto sin formato.
* i. Cree un aviso que advierta a todo el que acceda al dispositivo que el acceso no autorizado está prohibido.
* j. Guardar la configuración en ejecución en el archivo de configuración de inicio
* k. Configure el reloj en el router.
```swift
Router>en
Router#conf t
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secre
R1(config)#enable secret class   # {} Colocar clave al modo EXEC
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)# line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service password-encryption 
R1(config)#banner motd $El Acceso no autorizado esta prohibido$
R1(config)#exit
R1#clock set 6:54:00 8 May 2024
R1#copy running-config startup-config 
```
#### Paso 3: Configurar los parámetros básicos para cada switch
* a.Asigne un nombre de dispositivo al switch.
* b. Inhabilite la búsqueda DNS para evitar que el router intente traducir los comandos mal introducidos como si fueran nombres de host.
* Asiigne class como la contraseña cifrada del modo EXEC privilegiado.
* d. Asigne cisco como la contraseña de la consola y habilite el inicio de sesión.
* e. Asigne cisco como la contraseña de vty y habilite el inicio de sesión.
* f. Cifre las contraseñas de texto sin formato.
* g. Cree un aviso que advierta a todo el que acceda al dispositivo que el acceso no autorizado está prohibido.
* h. Ajuste el reloj en el interruptor.
* i. Guardar la configuración en ejecución en la configuración de arranque
```swift
Switch>en
Switch#conf t
Switch(config)#hostname S1
S1(config)#no ip domain-lookup 
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption 
S1(config)#banner motd $EL acceso no autorizado esta prohibido$
S1(config)#exit
S1#clock set 7:04:00 8 May 2024
S1#copy running-config startup-config 
```
#### Paso 4: Configurar los equipos host
Consulte la tabla de direccionamiento para obtener información de direcciones de los equipos host.
### Parte 2: Crear redes VLAN y asignar puertos de switch
En la Parte 2, creará VLAN como se especifica en la tabla anterior en ambos awitches. A continuación, asignará las VLAN a la interfaz adecuada y verificará los valores de configuración. Complete las siguientes tareas en cada switch.
#### Paso 1: Crear las VLAN en los switches
* a. Cree y asigne un nombre a las VLAN necesarias en cada switch de la tabla anterior.
```swift
S1(config)# vlan 10
S1(config-vlan)# name Management
S1(config-vlan)# vlan 20
S1(config-vlan)# name Sales
S1(config-vlan)# vlan 999
S1(config-vlan)# name Parking_Lot
S1(config-vlan)# exit

S2(config)# vlan 10
S2(config-vlan)# name Management
S2(config-vlan)# vlan 30
S2 (config-vlan) # name Operations
S2(config-vlan)# vlan 999
S2(config-vlan)# name Parking_Lot
S2(config-vlan)# exit
```
* b. Configure la interfaz de administración y el gateway predeterminado en cada switch con la informaciónde dirección IP incluida en la tabla de direccionamiento.
```swift
S1(config)# interface vlan 10
S1(config-if)# ip address 192.168.10.11 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.10.1

S2(config)# interface vlan 10
S2(config-if)# ip address 192.168.10.12 255.255.255.0
S2(config-if)# no shutdown
S2(config-if)# exit
S2(config)# ip default-gateway 192.168.10.1
```
* c. Asigne todos los puertos no utilizados del switch a la VLAN de Parking_Lot, configúrelos para el modo deacceso estático y desactívalos administrativamente.
```swift
S1(config)# interface range f0/2 - 4 , f0/7 - 24 , g0/1 - 2
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 999
S1(config-if-range)# shutdown

S2 (config) # rinterface range f0/2 - 17, f0/19 - 24, g0/1 - 2
S2(config-if-range)# switchport mode access
S2(config-if-range)# switchport access vlan 999
S2(config-if-range)# shutdown
```
Nota: El comando interface range es útil para llevar a cabo esta tarea con los pocos comandos que seanecesario. 
#### Paso 2: Asignar las VLAN a las interfaces del switch correctas
* Asigne los puertos usados a la VLAN apropiada (especificada en la tabla VLAN anterior) y configúrelos para el modo de acceso estático.
```swift
S1(config)# interface f0/6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 20

S2(config)# interface f0/18
S2(config-if)# switchport mode access
S2(config-if)# switchport access vlan 30
```
* Verifique que las VLAN estén asignadas a las interfaces correctas.
```swift
S1# show vlan brief

VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/1, Fa0/5
10 Management active
20 Sales active Fa0/6
999 Parking_Lote activo Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gi0/1, Gi0/2
1000 Native active
1002 fddi-default act/unsup
1003 token-ring-default act/unsup
1004 fddinet-default act/unsup
1005 trnet-default act/unsup

```
```swift
S2# show vlan brief

VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/1
10 Management active
30 Operations active Fa0/18
999 Parking_Lote activo Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gi0/1, Gi0/2
1000 Native active
1002 fddi-default act/unsup
1003 token-ring-default act/unsup
1004 fddinet-default act/unsup
1005 trnet-default act/unsup
```
### Parte 3: Configurar un enlace troncal 802.1Q entre los switches
En la Parte 3, configurará manualmente la interfaz F0/1 como troncal.
#### Paso 1: Configure manualmente la interfaz troncal F0 / 1 en el interruptor S1 y S2.
* a. Configure la conexión troncal estática en la interfaz F0/1 para ambos switches.
```swift
S1(config)# interface f0/1
S1(config-if)# switchport mode trunk

S2(config)# interface f0/1
S2(config-if)# switchport mode trunk
```
* b. Establezca la VLAN nativa en 1000 en ambos switches.
```swift
S1(config)#vlan 1000
S1(config-vlan)#name Native
S1(config)#interface f0/1
S1(config-if)#switchport trunk native vlan 1000
```
```swift
S2(config)#vlan 1000
S2(config-vlan)#name Native
S2(config)#interface f0/1
S2(config-if)#switchport trunk native vlan 1000
```
* c. Especifique que las VLAN 10, 20, 30 y 1000 pueden cruzar el troncal.
```swift
S1(config-if)# switchport trunk allowed vlan 10,20,1000

S2(config-if)# switchport trunk allowed vlan 10,30,1000
```
* d. Verifique los puertos de enlace troncal, la VLAN nativa y las VLAN permitidas en el troncal.
```swift
S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       10,20,1000

Port        Vlans allowed and active in management domain
Fa0/1       10,20,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       none

S1#
```
```swift
S2#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       10,30,1000

Port        Vlans allowed and active in management domain
Fa0/1       10,30,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       10,30,1000

S2#
```
#### Paso 2: Configurar manualmente la interfaz troncal de S1 F0 / 5
* a. Configure la interfaz F0/5 de S1 con los mismos parámetros de troncal que F0/1. Este es el troncal del router.
```swift
S1(config)#int f0/5
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
```
* b. Guardar la configuración en ejecución en el archivo de configuración de inicio
```swift
S1# copy running-config startup-config

 S2# copy running-config startup-config
```
* c. Verifique la conexión troncal.
¿Qué sucede si G0/0/1 en R1 está caído?
	* Del Switch 1 en el puerto  F0/5 no se mostrará si el estado de la interfaz G 0/0/1 en el router está inactivo.
### Parte 4: Configure el enrutamiento entre VLAN en el router
#### Paso 1: Configurar el router
* a. Active la interfaz G0/0/1 según sea necesario en el router.
```swift
R1(config)#int g0/0/1
R1(config-if)#no sh
```
* b. Configure las subinterfaces para cada VLAN como se especifica en la tabla de direcciones IP. Todas las subinterfaces utilizan encapsulación 802.1Q. Asegúrese de que la subinterfaz de la VLAN nativa no tenga asignada una dirección IP. Incluya una descripción para cada subinterfaz.
```swift
R1 (config) # interface g0/0/1.10
R1 (config-subif) # description Management Network
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1 (config-subif) # interface g0/0/1.20
R1 (config-subif) # description Sales Network
R1(config-subif)# encapsulation dot1q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1 (config-subif) # interface g0/0/1.30
R1 (config-subif) # description Operations Network
R1(config-subif)# encapsulation dot1q 30
R1(config-subif)# ip address 192.168.30.1 255.255.255.0
R1 (config-subif) # interface g0/0/1.1000
R1(config-subif)# description Native VLAN
R1 (config-subif) # encapsulation dot1q 1000 native
```
* c. Verifique que las subinterfaces estén operativas
```swift
R1(config-subif)#do show ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.10192.168.10.1    YES manual up                    up 
GigabitEthernet0/0/1.20192.168.20.1    YES manual up                    up 
GigabitEthernet0/0/1.30192.168.30.1    YES manual up                    up 
GigabitEthernet0/0/1.1000unassigned      YES unset  up                    up 
Vlan1                  unassigned      YES unset  administratively down down
R1(config-subif)#
```
### Parte 5: Verifique que el enrutamiento entre VLAN esté funcionando
#### Paso 1: Complete las siguientes pruebas de PC-A. Todo debería tener éxito.
**Nota:** Es posible que tenga que deshabilitar el firewall de PC para que funcionen los pings
	a. Haga ping desde la PC-A a su puerta de enlace predeterminada.
		* Funciona y llega
	b. Emitir un comando ping de PC-A a PC-B
		* No hace ping, ya que segun la tabla de VLAN SE ESPECIFICA QUE LA VLAN 20 solo esta en el S1 y la VLAN 30 solo esta en el S2
	c. Haga ping desde la PC-A a la S2
		El ping funcion
#### Paso 2: Complete la siguiente prueba de PC-B
Desde la ventana Símbolo del sistema en PC-B, ejecute el comando tracert a la dirección de PC-A.
* ¿Qué direcciones IP intermedias se muestran en los resultados?
	* No muestra nada por   VLAN 20 solo esta en el S1 y la VLAN 30 solo esta en el S2

