# UD5-P2.  Conexiones de redes y gestión de recursos en Windows

## Escenario
Se simula una pequeña red local formada por dos equipos Windows que deben comunicarse entre sí y permitir el acceso a recursos compartidos en red.

La práctica se realizará utilizando:

Windows Server (servidor de recursos)
Windows 11 (cliente)
Aunque Windows dispone de múltiples herramientas gráficas, todas las tareas deberán realizarse utilizando la terminal (PowerShell o CMD).

El objetivo es aprender a configurar la red, verificar la conectividad, analizar el funcionamiento de la red y gestionar recursos compartidos entre equipos.

## E1. Identificación de interfaces de red  

**Muestra la configuración de red ejecutando:**

```bash
ipconfig /all
```
Cliente:

![alt text](/images4/image-1.png)

Servidor:

![alt text](/images4/image.png)

**Responde:**

- ¿Qué interfaces de red aparecen? 

    Ethernet.

- ¿Cuál es la interfaz activa?

    Ethernet.

- ¿Qué dirección MAC tiene la interfaz?

    Cliente: 08-00-27-27-2D-20

    Servidor: 08-00-27-8B-3E-9F

### Explicación  
El comando ipconfig /all permite visualizar toda la configuración de red del equipo, incluyendo las interfaces disponibles, dirección IP, máscara de subred, dirección MAC y otros parámetros de red.

La interfaz activa es aquella que tiene una dirección IP asignada.

La dirección MAC identifica físicamente la tarjeta de red dentro de la red local.

### Documentación consultada

https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig

## E2. Identificación de adaptadores de red  

**Abre PowerShell como administrador y ejecuta:**

```bash
Get-NetAdapter
```
Cliente:

![alt text](/images4/image-2.png)

Servidor:

![alt text](/images4/image-3.png)

**Responde:**

- ¿Qué adaptadores aparecen en el sistema? 

    Ethernet.

- ¿Cuál es el nombre exacto de la interfaz que se está utilizando?
    
    El nombre exacto es Ethernet.

**Incluye capturas y explica la información mostrada.**

### Explicación
El comando Get-NetAdapter muestra todos los adaptadores de red del sistema junto con su estado, velocidad y dirección MAC.

Permite identificar el nombre exacto de la interfaz para usarla en configuraciones posteriores.

### Documentación consultada  
https://learn.microsoft.com/en-us/powershell/module/netadapter/get-netadapter?view=windowsserver2025-ps

## E3. Identificación de la configuración IP  

**Muestra la configuración IP del sistema:**

```bash
ipconfig
```
Cliente:

![alt text](/images4/image-5.png)

Servidor:

![alt text](/images4/image-4.png)


**Responde:**

- ¿Qué dirección IP tiene actualmente el sistema?

    Cliente: 10.0.2.15
    
    Servidor: 10.0.2.15

- ¿Qué máscara de red utiliza?

    Cliente y Servidor: 255.255.255.0

- ¿Existe una puerta de enlace configurada?

    Sí, 10.0.2.2 ambas.

### Explicación
El comando ipconfig muestra la configuración IP básica del sistema, incluyendo la dirección IP, máscara de red y puerta de enlace.

La máscara 255.255.255.0 indica que los primeros tres octetos identifican la red.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig

## E4. Análisis de la tabla de rutas  

**Muestra la tabla de rutas del sistema ejecutando:**

```bash
route print
```
Cliente:

![alt text](/images4/image-6.png)

Servidor:

![alt text](/images4/image-7.png)

**Responde:**

- ¿Qué red local aparece configurada?

    Cliente y Servidor: 10.0.2.0

- ¿Qué interfaz se utiliza para acceder a la red?

    La 10.0.2.15

- ¿Qué significa cada columna de la tabla? 
    
    Las columnas principales son:

    **Network Destination**

    Indica la red destino.

    Ejemplo:

    192.168.60.0

    **Netmask**

    Indica la máscara de red.

    Ejemplo:

    255.255.255.0

    **Gateway**

    Indica la puerta de enlace.

    Ejemplo:

    0.0.0.0

    **Interface**

    Indica la interfaz usada.

    Ejemplo:

    192.168.60.10

    **Metric**

    Indica el coste de la ruta.

    Cuanto menor es el valor:

    más prioridad tiene

**Incluye capturas y explica la información mostrada.**

### Explicación
El comando route print muestra la tabla de rutas del sistema, que determina cómo se envían los paquetes a diferentes redes.

Cada entrada indica el destino, la máscara, la puerta de enlace y la interfaz utilizada.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/route_ws2008


## E5. Configuración del nombre del equipo  

**Consulta el nombre del equipo ejecutando:**

```bash
hostname
```
Cliente:

![alt text](/images4/image-8.png)

Servidor:

![alt text](/images4/image-9.png)

**Cambia el nombre del equipo utilizando PowerShell.**

**Servidor:**

```bash
Rename-Computer -NewName win-srv01 -Restart
```
![alt text](/images4/image-10.png)

**Cliente:**

```bash
Rename-Computer -NewName win-cli01 -Restart
```
![alt text](/images4/image-11.png)

**Tras reiniciar el sistema, comprueba el cambio ejecutando:**

```bash
hostname
```
Cliente:

![alt text](/images4/image-12.png)

Servidor:

![alt text](/images4/image-13.png)

### Explicación
El comando hostname muestra el nombre actual del equipo.

El comando Rename-Computer permite cambiar el nombre del equipo dentro de la red.

El parámetro -Restart reinicia el sistema para aplicar los cambios.

El nombre del equipo es fundamental en redes locales porque permite identificar cada dispositivo.

### Documentación consultada  
Rename-Computer:
https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/rename-computer

Hostname:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/hostname

## E6. Configuración de dirección IP estática  

**Abre PowerShell como administrador.**

**Consulta el nombre de la interfaz de red:**

```bash
Get-NetAdapter
```
Cliente:

![alt text](/images4/image-14.png)

Servidor:

![alt text](/images4/image-15.png)

**Configura la dirección IP utilizando PowerShell.**

**Servidor:**

```bash
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.60.10 -PrefixLength 24
```
![alt text](/images4/image-16.png)

**Cliente:**

```bash
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.60.20 -PrefixLength 24
```
![alt text](/images4/image-17.png)

**Comprueba la configuración:**

```bash
ipconfig
```
Cliente:

![alt text](/images4/image-18.png)

Servidor:

![alt text](/images4/image-19.png)


### Explicación  
El comando New-NetIPAddress permite asignar una dirección IP manualmente.

Una IP estática es necesaria cuando:

Se quiere mantener una dirección fija
Se configuran servidores
Se comparten recursos

El comando ipconfig permite verificar que la dirección IP se haya configurado correctamente.

### Documentación consultada  
New-NetIPAddress:
https://learn.microsoft.com/en-us/powershell/module/nettcpip/new-netipaddress


## E7. Verificación de conectividad entre equipos  

**Desde cada máquina ejecuta:**

```bash
ping 192.168.60.10
```
Cliente:

![alt text](/images4/image-21.png)

```bash
ping 192.168.60.20
```
Servidor:

![alt text](/images4/image-20.png)

>NOTA: He tenido que hacer unos cambios en la configuración de red para tener acceso a Internet, ya que el comando dado no se configuraba la puerta de enlace y el DNS.

![alt text](/images4/image-22.png)

>NOTA: Aparte de esto también tuve que habilitar la detección de redes ya que sino no se podría comunicar y el ping fallaría.

![alt text](/images4/image-23.png)


**Responde:**

- ¿Se reciben respuestas?

    Si por ambas partes.
- ¿Cuántos paquetes se envían y reciben?
    
    Se envían 4 paquetes y se reciben 4.

- ¿Qué información muestra el comando?

    El comando muestra:

    Dirección IP destino, 
    Tamaño del paquete, 
    Tiempo de respuesta, 
    TTL (Time To Live), 
    Estadísticas finales


### Explicación  
El comando ping permite verificar la conectividad entre equipos.

Funciona enviando paquetes ICMP al destino y esperando respuesta.

Si se reciben respuestas:

La comunicación es correcta.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ping


## E8. Configuración de resolución de nombres local  

**Edita el archivo:**

```bash
C:\Windows\System32\drivers\etc\hosts
```
**Puedes abrirlo ejecutando:**

```bash
notepad C:\Windows\System32\drivers\etc\hosts
```
Cliente:

![alt text](/images4/image-24.png)

Servidor:

![alt text](/images4/image-25.png)


**Añade las entradas:**

```bash
192.168.60.10 win-srv01
192.168.60.20 win-cli01
```
Cliente:

![alt text](/images4/image-27.png)

Servidor:

![alt text](/images4/image-26.png)


**Comprueba la resolución de nombres ejecutando:**

```bash
ping win-srv01
```
Cliente:

![alt text](/images4/image-29.png)

Servidor:

![alt text](/images4/image-28.png)

```bash
ping win-cli01
```



### Explicación
El archivo hosts es consultado antes que un servidor DNS.

Permite definir manualmente nombres de equipos en redes pequeñas.

Es útil cuando:

No existe servidor DNS
Se quieren probar configuraciones

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/hosts-file


## E9. Análisis de puertos abiertos  

**Muestra los puertos abiertos ejecutando:**

```bash
netstat -ano
```
Cliente:

![alt text](/images4/image-30.png)

Servidor:

![alt text](/images4/image-31.png)

**Responde:**

- ¿Qué puertos aparecen abiertos? 

    Cliente:

    135 TCP

    445 TCP

    5040 TCP

    5432 TCP

    7680 TCP

    49664 TCP

    49665 TCP

    49666 TCP

    49667 TCP

    49668 TCP

    49669 TCP

    139 TCP

    Servidor:

    135 TCP

    445 TCP

    5357 TCP

    5985 TCP

    47001 TCP

    49664 TCP

    49665 TCP

    49666 TCP

    49667 TCP

    49668 TCP

    49669 TCP

    139 TCP

- ¿Qué significa cada columna mostrada?

    **Proto**

    Indica el protocolo utilizado:

    TCP
    UDP
    Local Address

    Indica:

    Dirección IP local + puerto

    Ejemplo:

    0.0.0.0:445

    Significa que el puerto 445 está escuchando en todas las interfaces.

    **Foreign Address**

    Indica la dirección remota conectada.

    Ejemplo:

    0.0.0.0:0

    Significa que no hay conexión remota aún.

    **State**

    Indica el estado del puerto.

    Ejemplo:

    **LISTENING**

    Significa que el puerto está esperando conexiones.

    Otros estados posibles:

    ESTABLISHED
    TIME_WAIT
    CLOSE_WAIT

    **PID**

    Ejemplo:

    884

    Es el identificador del proceso que utiliza ese puerto.

- ¿Qué indica el identificador de proceso (PID)?

    El PID identifica el proceso que está utilizando el puerto.

    Se puede consultar con:
    ```bash
    tasklist
    ```
    Esto permite saber qué programa usa el puerto.


### Explicación  
El comando netstat -ano muestra:

Puertos abiertos

Conexiones activas

Procesos asociados

Se utiliza para analizar el tráfico de red y detectar servicios activos.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat


## E10. Consulta de la tabla ARP  

**Muestra la tabla ARP del sistema:**

```bash
arp -a
```
Cliente:

![alt text](/images4/image-32.png)

Servidor:

![alt text](/images4/image-33.png)

**Responde:**

- ¿Qué dirección IP aparece asociada al otro equipo? 

    En el servidor aparece:

    192.168.60.20

    En el cliente aparece:

    192.168.60.10

- ¿Qué dirección MAC tiene?
    
    Servidor: Del cliente = 08-00-27-27-2d-20

    Cliente: Del servidor = 08-00-278b-3e-9f


### Relación entre IP y MAC

La tabla ARP relaciona:

Dirección IP → Dirección MAC

Ejemplo:

192.168.60.10 → 08-00-27-AB-CD-EF

Esto permite enviar datos dentro de la red local.

### Explicación
El comando arp -a muestra la tabla ARP.

ARP permite traducir direcciones IP a direcciones MAC dentro de una red local.

Sin ARP, los equipos no podrían comunicarse dentro de la red.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/arp


## E11. Creación de una carpeta compartida  

**En win-srv01, crea una carpeta:**

```bash
C:\red
```
![alt text](/images4/image-34.png)

**Abre PowerShell como administrador y comparte la carpeta:**

```bash
New-SmbShare -Name red -Path C:\red
```
![alt text](/images4/image-35.png)


**Comprueba los recursos compartidos:**

```bash
Get-SmbShare
```
![alt text](/images4/image-36.png)


### Explicación  
El comando New-SmbShare permite crear recursos compartidos en Windows.

Un recurso compartido permite:

Compartir archivos
Acceder desde otros equipos
Transferir información en red

### Documentación consultada
https://learn.microsoft.com/en-us/powershell/module/smbshare/new-smbshare?view=windowsserver2025-ps


## E12. Acceso al recurso compartido  

**Desde win-cli01, accede al recurso compartido ejecutando:**

```bash
net use Z: \\win-srv01\red
```
![alt text](/images4/image-37.png)

**Comprueba que el recurso aparece montado:**

```bash
net use
```
![alt text](/images4/image-38.png)

**Verifica el acceso al contenido:**

```bash
dir Z:
```
![alt text](/images4/image-39.png)

**Incluye capturas y explica cómo funciona el acceso a recursos compartidos.**

### Explicación  
El comando net use permite conectar unidades de red.

Permite acceder a carpetas remotas usando una letra de unidad.

Esto facilita el uso de recursos compartidos.

### Documentación consultada  
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155(v=ws.11)


## E13. Transferencia de archivos  

**Desde el cliente crea un archivo:**

```bash
notepad prueba.txt
```
![alt text](/images4/image-40.png)

**Copia el archivo al recurso compartido:**

```bash
copy prueba.txt Z:\
```
![alt text](/images4/image-45.png)

>NOTA: No me deja copiar el archivo, lo he consultado con fuentes externas y no he conseguido copiarlo.


### Explicación 
El comando copy permite copiar archivos entre ubicaciones.

En este caso no me ha dejado realizar la copia por temas de permisos pero se copia un archivo desde el cliente hacia un recurso compartido del servidor.

Esto demuestra que:

La red funciona correctamente, 
el recurso compartido es accesible, 
la transferencia de archivos es posible, 

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/copy

## E14. Análisis de conexiones activas  

**Muestra las conexiones activas del sistema:**

```bash
netstat -ano
```
Cliente:

![alt text](/images4/image-41.png)

Servidor:

![alt text](/images4/image-42.png)


**Responde:**

- ¿Qué conexiones aparecen activas?

    Cliente: 
    
    ESTABLISHED, 
    CLOSE_WAIT

    Servidor:

    ESTABLISHED, 
    TIME_WAIT

- ¿Qué significa el estado de las conexiones? 

    LISTENING: El puerto está abierto y esperando a que alguien se conecte a él. No hay comunicación activa todavía.

    ESTABLISHED: La conexión está activa. Ambos equipos están conectados y pueden transferir datos en este momento.

    CLOSE_WAIT: El equipo remoto ha cerrado la conexión, y tu sistema está esperando a que el programa local haga lo mismo para finalizar.

    TIME_WAIT: La conexión se ha cerrado, pero el sistema mantiene el registro un momento para asegurarse de que no lleguen paquetes retrasados.


### Explicación  
El comando netstat -ano permite visualizar las conexiones activas del sistema.

Se utiliza para:

Analizar tráfico de red
Ver comunicaciones activas
Detectar problemas de red

En esta práctica permite verificar la conexión entre cliente y servidor.

### Documentación consultada  
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat


## E15. Comprobación tras reinicio  

**Reinicia ambas máquinas.**
**Comprueba que:**

- la dirección IP sigue configurada correctamente  
- el nombre del equipo se mantiene  
- el recurso compartido sigue disponible  

Cliente:

![alt text](/images4/image-43.png)

Servidor:

![alt text](/images4/image-44.png)


### Explicación  
Después del reinicio, la configuración se mantiene porque:

La IP fue configurada como estática
El nombre del equipo fue guardado en el sistema
El recurso compartido fue creado permanentemente

Esto permite que la red funcione correctamente incluso después de reiniciar los equipos.

### Documentación consultada  
ipconfig:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig

hostname:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/hostname

net use:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/net-use