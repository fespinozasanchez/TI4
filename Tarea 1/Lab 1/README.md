

# Lab 1.1.7 configuración Básica
![](https://beta.appflowy.cloud/api/file_storage/5797f5ae-32d4-4fc8-b351-ff49f1e56fb9/blob/12162130929303095011.png)
### Parte 1: Tender el cableado de red y verificar la configuración predeterminada del switch
1. ¿Por qué debe usar una conexión de consola para configurar inicialmente el switch? 
	1. Para poder configurar todo el equipo de acuerdo a nuestra necesidad
2. ¿Por qué no esposible conectarse al switch a través de Telnet o SSH?
	1. Primeramente porque no estamos conectado por ethernet con algun cable rj45 y segundo porque debemos configurar las credenciales (Claves, secretas, usuarios, etc...) para la conexión por ssh
3. ¿Cuántas interfaces FastEthernet tiene un switch 2960?
	1. Segun lo revisado con `sh run` no hay interfaces FastEthernet
4. ¿Cuántas interfaces Gigabit Ethernet tiene un switch 2960?
	1. Tiene 28 interfaces Gigabit Ethernet: 24 -> 1/0/24 y 4 de 1/1/4
5. ¿Cuál es el rango de valores que se muestra para las líneas vty? l El "rango de valores") especifica cuántas sesiones simultáneas o conexiones remotas pueden establecerse con el dispositivo.
	1. line vty 0 4 —> Tiene un rango de 0 a 4 
6. Examine el archivo de configuración de inicio en la NVRAM.  ¿Por qué aparece este mensaje?
	1. `startup-config` >> is not present. Porque no hay ninguna configuracion guardada para la NVRAM
##### Examine las características de la SVI para la VLAN 1.
1.  ¿Hay alguna dirección IP asignada a VLAN 1?
	1. No hay dirección ip asignada  > >` show int vlan 1`
2.  ¿Cuál es la dirección MAC de esta SVI? Las respuestas varían.
	1. Tenemos una dirección MAC:` 0060.2fde.172d` y una Dirección MAC  del chipset  `0060.2fde.172d`
3.  ¿Está activa esta interfaz?
	1. No, esta activa ya que no le tenemos ninguna interfaz conectada, ademas de encender con un` no sh` la int vlan 1
4.  Examine las propiedades IP de la VLAN 1 SVI. i. ¿Qué resultado ve? 
	`conf t`
	`int vlan 1 `
	`no sh`
	`show ip int vlan 1`
		1. Vlan1 is up, line protocol is down
		Internet protocol processing disabled  
#####  Conecte un cable Ethernet desde PC-A al puerto 6 del switch y examine las propiedades de IP de la SVI VLAN 1. Aguarde un momento para que el switch y la computadora negocien los parámetros de dúplex y velocidad. 
1. ¿Qué resultado ve?
	1. Vlan1 is up, line protocol is up/ Internet protocol processing disable.
###### Examine la información de la versión del IOS de Cisco del switch, PREGUNTAS:
1. ¿Cuál es la versión del IOS de Cisco que está ejecutando el switch?
	1. `show version`: 16.3.2
1. ¿Cuál es el nombre del archivo de imagen del sistema?
	1. `show version`: CAT3K_CAA-UNIVERSALK9
1. ¿Cuál es la dirección MAC base de este switch?
	1. `show version`:` 00:60:2F:DE:17:2D`
##### Examine las propiedades predeterminadas de la interfaz FastEthernet que usa la PC-A.
Pregunta:
1. ¿La interfaz está activa o inactiva?
	1. `show int g1/0/6` : GigabitEthernet1/0/6 is up, line protocol is up (connected)
1. ¿Qué haría que una interfaz se active?
	1. Utilizar la interfaz, o sea conectar un dispositivo a esta interfaz
1. ¿Cuál es la dirección MAC de la interfaz?
	1. `show int g1/0/6` :` 000c.8589.1806`
1. ¿Cuál es la configuración de velocidad y de dúplex de la interfaz?
	1. `show int g1/0/6` : Full-duplex, 100Mb/s
##### ** Examine la configuración VLAN configuración del switch.**
1. ¿Qué puertos están en VLAN 1?
	1. `show vlan` :  Gi1/0/1 - Gi1/0/24 y Gi1/1/0 - Gi1/1/4
1. ¿La VLAN 1 está activa?
	1. Si está activa
1. ¿Qué tipo de VLAN es la VLAN predeterminada?
	1. `enet` -> de tipo ethernet

**Examine la memoria flash. Ejecute uno de los siguientes comandos para examinar el contenido del directorio flash. Los archivos poseen una extensión, tal como .bin, al final del nombre del archivo. Los directorios no tienen una extensión de archivo.**
`show flash`
`dir flash`
**
Pregunta:**
1. ¿Cuál es el nombre de archivo de la imagen de IOS de Cisco?
	    3  -rw-   505532849          <no date>  cat3k_caa-universalk9.16.03.02.SPA.bin
	    4  -rw-         616          <no date>  vlan.dat
### 
Parte 2: Configurar los parámetros básicos de los dispositivos de red
* Copie la siguiente configuración básica y péguela en S1 mientras se encuentre en el modo de configuración global. 
```bash
no ip domain-lookup                                                                           # Desactivamos la Traducción  de nombres a direcciones de dispositivos 
hostname S1                                                                                           # Cambiamos el nombre del switch
service password-encryption                                                            # Con esto encriptamos todas las contraseñas de forma global
enable secret class                                                                               # Activamos un algoritmo fuerte de encriptación MD5  Para el modo privilegiado EXEC
banner motd #Unauthorized access is strictly prohibited. #     # Con esto solo se emite un mensaje por si no no hemos logeado
```
 
* Establezca la dirección IP de la SVI del switch. Esto permite la administración remota del switch.
	* Antes de poder administrar el S1 en forma remota desde la PC-A, debe asignar una dirección IP al switch. El switch está configurado de manera predeterminada para que la administración de este se realice a través de VLAN 1. Sin embargo, la práctica recomendada para la configuración básica del switch es cambiar la VLAN de administración a otra VLAN distinta de la VLAN 1
	* Con fines de administración, utilice la VLAN 99. La selección de la VLAN 99 es arbitraria y de ningunamanera implica que siempre deba usar la VLAN 99.
	* Primero, cree la nueva VLAN 99 en el switch. Luego, establezca la dirección IP del switch en 192.168.1.2con la máscara de subred 255.255.255.0 en la interfaz virtual interna VLAN 99. La dirección IPv6también se puede configurar en la interfaz SVI. Utilice las direcciones IPv6 que figuran en la tabla dedireccionamiento.
	* Observe que la interfaz VLAN 99 está en estado down, aunque haya introducido el comando noshutdown. Actualmente, la interfaz se encuentra en estado down debido a que no se asignaron puertosdel switch a la VLAN 99.
```swift
S1(config)#     int vlan 99                                                  # Ingresar a la int de vlan 99
S1(config-if)# ip address 192.168.1.2 255.255.255.0    # Asignar la ipv4/24
S1(config-if)# ipv6 add 2001:db8:acad:1::2/64            # Asignar la ipv6/64
S1(config-if)# ipv6 add fe80::2 link-local                      # Asignar la link local
S1(config-if)#no sh                                                              # Encender la interfaz
```

* Asigne todos los puertos de usuario a VLAN 99
```swift
S1(config)#int range g1/0/1-24                                         # El range nos ayudara a modificar todas las vlan que queramos en un rango
S1(config-if-range)#switchport acce
S1(config-if-range)#switchport access vlan 99           # colocar la vlan 99 en modo access
```
* Emita el comando show vlan brief verificar que todos los puertos estén en VLAN 99
```swift
S1(config-if-range)# do show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                              active    Gig1/1/1, Gig1/1/2, Gig1/1/3, Gig1/1/4
99   VLAN0099                      active    Gig1/0/1, Gig1/0/2, Gig1/0/3, Gig1/0/4
                                                                Gig1/0/5, Gig1/0/6, Gig1/0/7, Gig1/0/8
                                                                Gig1/0/9, Gig1/0/10, Gig1/0/11, Gig1/0/12
                                                               Gig1/0/13, Gig1/0/14, Gig1/0/15, Gig1/0/16
                                                               Gig1/0/17, Gig1/0/18, Gig1/0/19, Gig1/0/20
                                                               Gig1/0/21, Gig1/0/22, Gig1/0/23, Gig1/0/24
1002 fddi-default                 active    
1003 token-ring-default    active    
1004 fddinet-default           active    
1005 trnet-default                active  
```
* Configure el gateway predeterminado para S1. Si no se estableció ningún gateway predeterminado, no se puede administrar el switch desde una red remota que esté a más de un router de distancia. Aunque esta actividad no incluye un gateway IP externo, se debe tener en cuenta que finalmente conectará la LAN a un router para tener acceso externo. Si suponemos que la interfaz de LAN en el router es 192.168.1.1, establezca el gateway predeterminado para el switch.

```swift
S1(config)# ip default-gateway 192.168.1.1                     #Asignar el gateway por defecto
```

* El acceso al puerto de la consola también debe restringirse con una contraseña. Utilice cisco como contraseña de inicio de sesión de la consola en esta actividad. La configuración predeterminada permite todas las conexiones de consola sin necesidad de introducir una contraseña. Para evitar que los mensajes de consola interrumpan los comandos, use la opción logging synchronous.
```swift
S1(config)#line con 0                                              # Acceder al puerto de consola 0, porque solo es uno
S1(config-line)#password cisco                          # Asignar una contraseña `cisco`
S1(config-line)#logging synchronous               # Evitar que los mensajes de consola interrumpan los comandos
S1(config-line)#login                                              # Permitir login
```
* Configure las líneas de terminal virtual (vty) para el switch para permitir el acceso telnet. Si no configura una contraseña vty, no podrá hacer telnet al switch.
```swift
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login 
```
* ¿Por qué se requiere el comando login?
	* Para obligar a que los usuarios se logeen

	### Paso 2: Configurar una dirección IP en la PC-A.* Asigne a la computadora la dirección IP y la máscara de subred que se muestran en la tabla de direccionamiento. Aquí se describe una versión abreviada del procedimiento. No se requiere una puerta de enlace predeterminada para esta topología; sin embargo, puede ingresar 192.168.1.1 and fe80::1 para simular un enrutador conectado a S1.
	* 1) Navega hasta el Panel de control..
	* 2) En la vista Categoría, seleccione Ver el estado y las tareas de la red.
	* 3) Haga clic en Cambiar la configuración del adaptador en el panel izquierdo.
	* 4) Haga clic con el botón derecho en una interfaz Ethernet y elija Propiedades.
	* 5) Elija el Protocolo de Internet versión 4 (TCP / IPv4) y haga clic en Propiedades.
	* 6) Haga clic en el botón de opción Usar la siguiente dirección IP e ingrese la dirección IP y la
	máscara de subred y haga clic en Aceptar.
	* 7) Seleccione Protocolo de Internet versión 6 (TCP / IPv6) y haga clic en Propiedades.
	* 8) Haga clic en el botón de opción Usar la siguiente dirección IPv6 e ingrese la dirección IPv6 y el
	prefijo y haga clic en Aceptar para continuar
	* 9) Haga click en Aceptar para salir de la ventana Propiedades.

	### Parte 3: Verificar y probar la conectividad de red
	En la parte 3, verificará y registrará la configuración del switch, probará la conectividad de extremo a extremo entre la PC-A y el S1, y probará la capacidad de administración remota del switch.
	```
	S1#ping 192.168.1.10
	
	Type escape sequence to abort.
	Sending 5, 100-byte ICMP Echos to 192.168.1.10, timeout is 2 seconds:
	!!!!!
	Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
	```
	#### Paso 1: Mostrar la configuración del switch
	Use la conexión de la consola en PC-A para mostrar y verificar la configuración del switch. El comando show run muestra la configuración en ejecución completa, de a una página por vez. Utilice la barra espaciadora para avanzar por las páginas.
	```swift
	S1#show run
	Building configuration...
	
	Current configuration : 2380 bytes
	!
	version 16.3.2
	no service timestamps log datetime msec
	no service timestamps debug datetime msec
	service password-encryption
	!
	hostname S1
	!
	!
	enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
	!

	ip cef
	no ipv6 cef
	!
	!

	no ip domain-lookup
	!
	!
	spanning-tree mode pvst
	!

	interface GigabitEthernet1/0/1
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/2
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/3
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/4
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/5
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/6
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/7
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/8
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/9
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/10
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/11
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/12
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/13
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/14
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/15
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/16
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/17
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/18
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/19
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/20
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/21
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/22
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/23
	 switchport access vlan 99
	!
	interface GigabitEthernet1/0/24
	 switchport access vlan 99
	!
	interface GigabitEthernet1/1/1
	!
	interface GigabitEthernet1/1/2
	!
	interface GigabitEthernet1/1/3
	!
	interface GigabitEthernet1/1/4
	!
	interface Vlan1
	 no ip address
	!
	interface Vlan99
	 mac-address 0060.2fde.1701
	 ip address 192.168.1.2 255.255.255.0
	 ipv6 address FE80::2 link-local
	 ipv6 address 2001:DB8:ACAD:1::2/64
	!
	ip default-gateway 192.168.1.1
	ip classless
	!
	ip flow-export version 9
	!
	banner motd ^C Acceso Prhobido^C
	!

	line con 0
	 password 7 0822455D0A16
	 logging synchronous
	 login
	!
	line aux 0
	!
	line vty 0 4
	 password 7 0822455D0A16
	 login
	line vty 5 15
	 password 7 0822455D0A16
	 login
	!
	!
	!
	!
	end
	```
	*  Verifique la configuración de la VLAN 99 de administración.
	```
	Vlan99 is up, line protocol is up
	  Hardware is CPU Interface, address is 0060.2fde.1701 (bia 0060.2fde.1701)
	  Internet address is 192.168.1.2/24
	  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
	     reliability 255/255, txload 1/255, rxload 1/255
	  Encapsulation ARPA, loopback not set
	  ARP type: ARPA, ARP Timeout 04:00:00
	  Last input 21:40:21, output never, output hang never
	  Last clearing of "show interface" counters never
	  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
	  Queueing strategy: fifo
	  Output queue: 0/40 (size/max)
	  5 minute input rate 0 bits/sec, 0 packets/sec
	  5 minute output rate 0 bits/sec, 0 packets/sec
	     1682 packets input, 530955 bytes, 0 no buffer
	     Received 0 broadcasts (0 IP multicast)
	     0 runts, 0 giants, 0 throttles
	     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
	     563859 packets output, 0 bytes, 0 underruns
	     0 output errors, 23 interface resets
	     0 output buffer failures, 0 output buffers swapped out
	```

		* ¿Cuál es el ancho de banda en esta interfaz?
			* ? 100.000 Kbit
		* ¿Cuál es el estado de la VLAN 99?
			* Up
		* ¿Cuál es el estado del protocolo de línea?
			* Up
	#### Paso 2: Probar la conectividad de extremo a extremo con ping
	* a. Desde la línea de comandos en PC-A, emita el comando ping a la dirección de PC-A primero.
		```
		C:\>ping 192.168.1.10
		
		Pinging 192.168.1.10 with 32 bytes of data:
		
		Reply from 192.168.1.10: bytes=32 time<1ms TTL=128
		Reply from 192.168.1.10: bytes=32 time=15ms TTL=128
		Reply from 192.168.1.10: bytes=32 time=15ms TTL=128
		Reply from 192.168.1.10: bytes=32 time=3ms TTL=128
		
		Ping statistics for 192.168.1.10:
		    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
		Approximate round trip times in milli-seconds:
		    Minimum = 0ms, Maximum = 15ms, Average = 8ms
		```


	* En el símbolo del sistema de la PC-A, haga ping a la dirección de administración de SVI del S1.
		```
		C:\>ping 192.168.1.2
		
		Pinging 192.168.1.2 with 32 bytes of data:
		
		Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
		Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
		Reply from 192.168.1.2: bytes=32 time=1ms TTL=255
		Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
		
		Ping statistics for 192.168.1.2:
		    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
		Approximate round trip times in milli-seconds:
		    Minimum = 0ms, Maximum = 1ms, Average = 0ms
		```Debido a que la PC-A debe resolver la dirección MAC del S1 mediante ARP, es posible que se agote el tiempo de espera del primer paquete. Si los resultados del ping siguen siendo incorrectos, resuelva los problemas de configuración de los parámetros básicos del dispositivo. Verifique el cableado físico y las direcciones lógicas.
	#### Paso 3: Probar y verificar la administración remota del S1

	Ahora utilizará Telnet para acceder al switch en forma remota. En esta práctica de laboratorio, la PC-A y el S1 se encuentran uno junto al otro. En una red de producción, el switch podría estar en un armario de cableado en el piso superior, mientras que la computadora de administración podría estar ubicada en la planta baja. En este paso, utilizará Telnet para acceder al switch S1 en forma remota mediante la dirección de administración de SVI. Telnet no es un protocolo seguro; sin embargo, lo usará para probar el acceso remoto. Con Telnet, toda la información, incluidos los comandos y las contraseñas, se envía durante la sesión como texto no cifrado. En las prácticas de laboratorio posteriores, usará SSH para acceder a los
	dispositivos de red en forma remota.
	* Abra Tera Term u otro programa de emulación de terminal con capacidad Telnet.
	* Seleccione el servidor Telnet y proporcione la dirección de administración SVI para utilizar a S1. La contraseña es cisco.
	* Después de introducir la contraseña cisco, quedará en la petición de entrada del modo EXEC del usuario. Acceda al modo EXEC con privilegios con el comando enable y suministre la contraseña secreta class.
	* Guarde la configuración.
	* Escriba exit para finalizar la sesión de Telnet.



	```swift
	Trying 192.168.1.2 ...Open Acceso Prhobido
	User Access Verification
	
	Password: ***** (cisco)
	S1>en
	Password: ***** (class)
	S1#copy running-config startup-config         # Configuración guardada en el archivo de inicio
	Destination filename [startup-config]? 
	Building configuration...
	[OK]
	S1#
	```### Parte 4: Administrar la tabla de direcciones MAC
En la Parte 4 determinará las direcciones MAC que el switch ha aprendido, configurará una dirección MAC estática en una interfaz del switch y luego retirará la dirección MAC estática de la interfaz.
#### Paso 1: Registrar la dirección MAC del host
Abra una línea de comandos en PC-A y emita el comando ipconfig /all para determinar y registrar las direcciones Capa 2 (físicas) del NIC.

```swift
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 000C.854D.C942
   Link-local IPv6 Address.........: FE80::20C:85FF:FE4D:C942
   IPv6 Address....................: 2001:DB8:ACAD:1::10
   IPv4 Address....................: 192.168.1.10
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: F80::1
                                     192.168.1.1
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-00-4D-9C-8D-00-0C-85-4D-C9-42
   DNS Servers.....................: ::  0.0.0.0
```
#### Paso 2: Determinar las direcciones MAC que el switch ha aprendido
*  Muestre las direcciones MAC con el comando show mac address-table.
```swift
S1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

  99    000c.854d.c942    DYNAMIC     Gig1/0/6
S1#
```
* ¿Cuántas direcciones dinámicas hay?
	* 1
* ¿Cuántas direcciones MAC hay en total?
	* 1
* ¿Coincide la dirección MAC dinámica con la dirección MAC de PC-A?
	* SI  000c.854d.c942 ==   000C.854D.C942

#### Paso 3: Enumerar las opciones del comando show mac address-table
* Muestre las opciones de la tabla de direcciones MAC.
```swift
S1#show mac address-table ?
  dynamic     dynamic entry type
  interface   List MAC adresses on a specific interface
  static      static entry type
  <cr>
S1#show mac address-table 
```
* ¿Cuántas opciones se encuentran disponibles para el comando show mac address-table?
	* 3
* Emita el comando show mac address-table dynamic para mostrar solo las direcciones MAC que se detectaron dinámicamente.
```swift
S1# show mac address-table dynamic
```

	* ¿Cuántas direcciones dinámicas hay?
		* 1
	* Observe la entrada de dirección MAC de PC-A. El formato de dirección MAC del comando es xxxx.xxxx.xxxx.
		* S1# show mac address-table address  # NO DISPONIBLE EN PACKET TRACER o comando malo 
	#### Paso 4: Configurar una dirección MAC estática
	* Limpie la tabla de direcciones MAC.
	* Para quitar las direcciones MAC existentes, use el comando clear mac address-table dynamic en el modo EXEC con privilegios.

	```swift
	S1#clear mac address-table dynamic 
	S1#show mac address-table 
	          Mac Address Table
	-------------------------------------------
	
	Vlan    Mac Address       Type        Ports
	----    -----------       --------    -----
	
	  99    000c.854d.c942    DYNAMIC     Gig1/0/6       # Se actualiza muy rapido
	S1
	```
	* Verifique que la tabla de direcciones MAC se haya eliminado.
		* S1# show mac address-table
		* ¿Cuántas direcciones MAC estáticas hay?
			* 0
		* ¿Cuántas direcciones dinámicas hay?
		* 1
	* Examine nuevamente la tabla de direcciones MAC
	Es muy probable que una aplicación en ejecución en la computadora ya haya enviado una trama por la NIC hacia el S1. Observe la tabla de direcciones MAC de nuevo en el modo EXEC con privilegios para ver si S1 ha vuelto a aprender la dirección MAC de PC-A.
		* S1# show mac address-table
		* ¿Cuántas direcciones dinámicas hay?
			* 1
		* ¿Por qué cambió esto desde la última visualización?
			* No cambio, es un ambiente simulado, asi que sucede muy rapido



	Si el S1 aún no volvió a detectar la dirección MAC de la PC-A, haga ping a la dirección IP de la VLAN 99 del switch desde la PC-A y, a continuación, repita el comando `show mac address-table.`
	*  Configure una dirección MAC estática.
	Para especificar a qué puertos se puede conectar un host, una opción es crear una asignación estática de la dirección MAC del host a un puerto.
	Configure una dirección MAC estática en F0/6 con la dirección que se registró para la PC-A en la parte 4, paso 1. La dirección MAC 0050.56BE.6C89 se usa solo como ejemplo. Debe utilizar la dirección MAC de PC-A, que es diferente de la que se da aquí como ejemplo.
	S1(config)# `mac address-table static 0050.56BE.6C89 vlan 99 interface fastethernet 0/6`
	* Verifique las entradas de la tabla de direcciones MAC.
	```
	S1(config)#mac address-table static 000c.854d.c942 vlan 99 interface GigabitEthernet 1/0/6
	S1(config)#do show mac add
	S1(config)#do show mac address-table
	          Mac Address Table
	-------------------------------------------
	
	Vlan    Mac Address       Type        Ports
	----    -----------       --------    -----
	
	  99    000c.854d.c942    STATIC      Gig1/0/6
	S1(config)#
	```

	* ¿Cuántas direcciones MAC hay en total?
		* 1
	* ¿Cuántas direcciones estáticas hay?
		* 1
	*  Elimine la entrada de MAC estática. Ingrese al modo de configuración global y elimine el comando escribiendo no delante de la cadena de comandos.
	**Nota: La dirección MAC 0050.56BE.6C89 se usa solo en el ejemplo. Utilice la dirección MAC para PC-A.**
	`S1(config)# no mac address-table static 0050.56BE.6C89 vlan 99 interface fastethernet 0/6`
	```swift
	S1(config)#no mac address-table static 000c.854d.c942 vlan 99 interface GigabitEthernet 1/0/6
	S1(config)#do show mac address-table
	          Mac Address Table
	-------------------------------------------
	
	Vlan    Mac Address       Type        Ports
	----    -----------       --------    -----
	
	  99    000c.854d.c942    DYNAMIC     Gig1/0/6
	S1(config)#
	```


	* Verifique que la dirección MAC estática se haya borrado.
	`S1# show mac address-table`
		* ¿Cuántas direcciones MAC estáticas hay en total?
			* 0
## Preguntas de Reflexión
1. ¿Por qué debería configurar la contraseña de vty del switch?
	1. Para segurida, si no cualquier podría entrar
1. . ¿Para qué se debe cambiar la VLAN 1 predeterminada a un número de VLAN diferente?
	1. El cambio que hicimos fue netamente por un tema de seguridad, ya que por defecto la vlan 1 es la administrativa.
1. ¿Cómo puede evitar que las contraseñas se envíen como texto no cifrado?
	1. Con el service  password-encryption.
4.  ¿Para qué se debe configurar una dirección MAC estática en una interfaz de puerto?
	1. Controlar qué dispositivos están autorizados para conectarse a ese puerto. Esto es útil en entornos donde la seguridad es una prioridad, como en redes corporativas, donde solo dispositivos autorizados deben tener acceso a la red.

### Extra
Determinar si se creo el archivo vlan.dat 

```swift
S1(config)#do show flash

System flash directory:
File  Length   Name/status
  3   505532849cat3k_caa-universalk9.16.03.02.SPA.bin
  4   616      vlan.dat
[505533465 bytes used, 1034042343 available, 1539575808 total]
1.50426e+06K bytes of processor board System flash (Read/Write)
```


















> Full Dúplex. Este término describe la transmisión y recepción de datos simultáneas a través de un canal. Un dispositivo que sea Full Dúplex es capaz de realizar transmisiones de datos de red bidireccionales al mismo tiempo. No va a tener que esperar y verificar si se está emitiendo en un sentido.
Ventajas: Tiene un mejor rendimiento al duplicar el uso del ancho de banda. Un ejemplo en la utilización del Full Dúplex es en un teléfono
---
> Half Dúplex. Este tipo de dispositivos solo pueden transmitir en una dirección a la vez. Con este modo, los datos pueden moverse en dos direcciones, pero no al mismo tiempo. Por tanto la comunicación es bidireccional, pero una a la vez. Esto, como podemos imaginar, es menos óptimo que el caso anterior. 
 Un ejemplo de modo de uso sería un walkie-talkie. Los dos pueden hablar, pero no al mismo tiempo.

