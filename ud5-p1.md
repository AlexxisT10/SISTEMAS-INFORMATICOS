# UD5-P1. Conexiones de redes y gestión de recursos en Linux

Escenario

Se simula una pequeña red local formada por dos equipos Linux que deben comunicarse entre sí y permitir la administración remota de un servidor.

La práctica se realizará utilizando:

    Ubuntu Server 24 (servidor)
    Ubuntu Desktop 24 (cliente)

Aunque el cliente dispone de entorno gráfico, todas las tareas deberán realizarse desde terminal.

El objetivo es aprender a configurar la red, verificar la conectividad, analizar el funcionamiento de la red y gestionar recursos accesibles a través de ella.
Infraestructura

Máquina 1 — servidor
sistema: Ubuntu Server 24
hostname: srv01
rol: servidor accesible por red y por SSH

Máquina 2 — cliente
sistema: Ubuntu Desktop 24
hostname: cli01
rol: equipo desde el que se realizan pruebas de conectividad y acceso remoto
Configuración de red

Ambas máquinas estarán conectadas a la misma red local.

Red:

192.168.50.0/24

Direcciones IP:

srv01 → 192.168.50.10
cli01 → 192.168.50.20

## E1. Identificación de interfaces de red

En cada máquina, muestra las interfaces de red disponibles ejecutando:

```bash
ip a
```
> Maquina cliente
![maquina cliente](./images2/image.png)

> Maquina servidor
![maquina servidor](./images2/image2.png)


**¿Que interfaz de red está activa?**

La interfaz activa es enp0s3, ya que es la que tiene la conexión a la red.

**¿Qué dirección IP tiene asignada actualmente?**

En la maquina cliente tiene la dirección IP: 
10.0.2.15/24

En la maquina servidor tiene la dirección IP: 
10.0.2.15/24

**¿Qué dirección MAC tiene la interfaz?**

Cliente: 08:00:27:21:bb:a8 brd ff:ff:ff:ff:ff:ff

Servidor: 08:00:27:04:23:4d brd ff:ff:ff:ff:ff:ff

**¿A qué red pertenece la dirección IP?**

Ambas pertencen a la red 10.0.2.0/24

> He consultado la documentación usando únicamente el manual del comando ip en Ubuntu, con el comando:
```bash
man ip
```
Esto me permitió entender cómo se muestran las interfaces de red, las direcciones IP y la información asociada a cada interfaz.

## E2. Identificación de la configuración de red

Muestra nuevamente la configuración IP:
```bash
ip addr
```
![maquina cliente](./images2/image3.png)
![maquina servidor](./images2/image4.png)


Muestra la tabla de rutas del sistema:
```bash
ip route
```

![maquina cliente](./images2/image5.png)
![maquina servidor](./images2/image6.png)

**¿Qué red local aparece configurada?**
Ambas aparecen con la red local: 10.0.2.0/24

**¿Qué interfaz se utiliza para acceder a esa red?**
enp0s3

**¿Existe una puerta de enlace configurada?**
**Incluye capturas y explica cada campo relevante que aparece en la información mostrada. Indica las fuentes utilizadas para comprender la información mostrada por estos comandos.**
![alt text](./images2/image7.png)

He usado el siguiente comando para consultar la puerta de enlace:
```bash
route -n
```
La puerta de enlace 10.0.2.2 es tu puente hacia el mundo exterior. Es el dispositivo al que tu equipo le entrega todo el tráfico destinado a Internet porque, por sí solo, no sabe cómo salir de tu red local. Sin esta dirección, estarías aislado y solo podrías comunicarte con los equipos que tienes justo al lado.

## E3. Configuración del nombre de host

**Consulta el nombre actual del sistema ejecutando:**
```bash
hostname
```
![alt text](image.png)
![alt text](image-1.png)

**Configura el nombre correspondiente.**

Servidor:
```bash
sudo hostnamectl set-hostname srv01
```
![alt text](image-4.png)
Cliente:
```bash
sudo hostnamectl set-hostname cli01
```
![alt text](image-2.png)

**Comprueba el cambio ejecutando:**
```bash
hostname
```
Cliente:

![alt text](image-3.png)

Servidor:

![alt text](image-5.png)

### Explicación comando "hostname"
![alt text](image-6.png)
>El comando hostname es básicamente el carné de identidad de tu equipo en la red. Sirve para consultar o cambiar el nombre que identifica a tu sistema y su dominio, permitiendo que otros dispositivos sepan exactamente quién eres. Es la herramienta principal para gestionar tu nombre de máquina y asegurar que tu equipo sea reconocible dentro de cualquier red local o de internet


## 4. Configuración de dirección IP estática
**Edita el archivo de configuración de red:**
```bash
sudo nano /etc/netplan/01-netcfg.yaml
```
Configura las direcciones IP según el esquema definido en el escenario.

**Aplica la configuración:**
```bash
sudo netplan apply
```

**Comprueba la configuración:**
```bash
ip a
```
Incluye capturas del archivo de configuración y del resultado del comando utilizado para comprobar la configuración.

Explica qué función cumple cada parámetro utilizado en el archivo de configuración.

Indica la documentación consultada sobre Netplan.