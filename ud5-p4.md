# UD5-P4. Integración de un sistema Linux en un dominio Active Directory

## Escenario
En esta práctica se integrará un sistema Linux (Ubuntu 24.04) en el dominio Active Directory configurado en la práctica anterior.

El objetivo es comprobar que un equipo Linux puede autenticarse contra un controlador de dominio Windows, permitiendo que los usuarios del dominio puedan iniciar sesión en el sistema Linux.

Para realizar la integración se utilizarán las herramientas realmd, SSSD y Kerberos, siguiendo como referencia la documentación oficial de Ubuntu:

https://ubuntu.com/server/docs/how-to/sssd/with-active-directory

Esta práctica permite observar la interoperabilidad entre sistemas Linux y Windows dentro de un mismo dominio, algo habitual en infraestructuras empresariales.

La actividad está relacionada con la gestión de recursos en red, autenticación centralizada y seguridad, aspectos incluidos en el RA6 del módulo.


## E1. Configuración inicial del sistema Linux

En la máquina Ubuntu:

**Cambia el nombre del equipo a:**  
**cli-linux01**

![alt text](/images6/image.png)

**Reinicia el sistema.**

**Comprueba el nombre del equipo ejecutando:**

```bash
hostname
```

![alt text](/images6/image-1.png)

### Explicación
Este equipo pasa a tener un nombre dentro de la red. Servirá como ordenador cliente conectado al servidor del dominio para que los usuarios puedan iniciar sesión.

### Documentación consultada
https://askubuntu.com/questions/1132933/unable-to-change-hostname-in-ubuntu-18-04-server-on-virtualbox

## E2. Comprobación de conectividad con el dominio

Comprueba que el sistema Linux puede comunicarse con el controlador de dominio.

**Ejecuta:**

```bash
ping srv-ad01
```

![alt text](/images6/image-2.png)

> NOTA: Para poder hacer ping anteriormente deberiaramos de haber puesto la direccion IP del servidor con su nombre de host en /etc/hosts.

![alt text](/images6/image-3.png)

> Ahora hace ping con el host.

**Comprueba también la resolución DNS del dominio:**

```bash
nslookup empresa.local
```
![alt text](/images6/image-4.png)

> NOTA: Para que el nslookup funcione correctamente he cambiado el DNS configurado en la carpeta /etc/resolv.conf y puse la IP del servidor, porque es donde ubuntu pregunta al hacer el nslookup.

![alt text](/images6/image-5.png)

> Funciona correctamente el nslookup.

### Explicación
Sirve para comprobar que el equipo puede comunicarse con el servidor y resolver nombres del dominio. DNS es importante porque sin él no se pueden encontrar los equipos del dominio.

### Documentación consultada
https://manpages.ubuntu.com/manpages/jammy/man1/nslookup.1.html



## E3. Instalación de los paquetes necesarios

Instala las herramientas necesarias para integrar Linux con Active Directory.

**Ejecuta:**

```bash
sudo apt update
```

![alt text](/images6/image-6.png)

```bash
sudo apt install realmd sssd sssd-tools libnss-sss libpam-sss adcli samba-common-bin oddjob oddjob-mkhomedir packagekit
```

![alt text](/images6/image-7.png)


### Explicación
Estos programas permiten que Linux pueda conectarse y usar usuarios del dominio de Windows.

SSSD permite usar usuarios del servidor.
Realmd facilita la conexión al dominio.

### Documentación consultada
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_authentication_and_authorization_in_rhel/index


## E4. Descubrimiento del dominio

Comprueba que el sistema Linux detecta correctamente el dominio.

**Ejecuta:**

```bash
realm discover empresa.local
```

![alt text](/images6/image-8.png)


### Explicación
empresa.local: Es el nombre del dominio que el sistema ha encontrado en la red.

type: kerberos: Indica que el método de autenticación principal que utiliza este dominio es Kerberos (el estándar de Active Directory).

realm-name: EMPRESA.LOCAL: Es el nombre del "reino" de Kerberos, que por convención siempre aparece en mayúsculas.

domain-name: empresa.local: El nombre del dominio DNS.

configured: no: Esto es clave. Significa que este equipo Linux aún no está unido al dominio. Ha encontrado el dominio, pero no forma parte de él.

server-software: active-directory: Indica que el servidor que controla este dominio es un servidor Windows con Active Directory.

client-software: sssd: Informa que el sistema utilizará SSSD como el software para gestionar la comunicación y la identidad con el dominio.

### Documentación consultada
https://www.freedesktop.org/software/realmd/docs/


## E5. Unión del equipo Linux al dominio

Une el sistema Ubuntu al dominio.

**Ejecuta:**

```bash
sudo realm join empresa.local -U Administrator
```
![alt text](/images6/image-9.png)

Introduce la contraseña del administrador del dominio cuando se solicite.

### Explicación
Esto conecta el ordenador al dominio. A partir de ese momento el equipo ya forma parte de la red y puede usar usuarios del servidor.

### Documentación consultada
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/windows_integration_guide/cmd-realmd


## E6. Verificación de la unión al dominio

Comprueba que el equipo se ha unido correctamente al dominio.

**Ejecuta:**

```bash
realm list
```

![alt text](/images6/image-10.png)

### Explicación
Unión confirmada: Lo más importante es que en configured ya sale como kerberos-member. Eso significa que la máquina ya está metida en el dominio empresa.local y no solo detectada.

Formato de login: En login-formats me indica que para entrar tengo que usar el formato usuario@empresa.local.

Política de acceso: La línea login-policy confirma que el equipo ya permite que cualquier usuario del dominio se loguee en esta máquina.

Software de gestión: Sigue usando sssd como cliente para conectar con el Active Directory y Kerberos para el tema de las contraseñas.

Dependencias: Los paquetes que salen abajo (adcli, samba-common-bin, etc.) ya están configurados y son los que hacen que la comunicación con el servidor de Windows funcione bien.

### Documentación consultada
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/windows_integration_guide/cmd-realmd



## E7. Comprobación de usuarios del dominio

Verifica que el sistema Linux reconoce los usuarios del dominio.

**Ejecuta:**

```bash
id usuario1@empresa.local
```

![alt text](/images6/image-11.png)


### Explicación
uid: El identificador único del usuario (usuario1) en el dominio.

gid: El identificador del grupo principal al que pertenece (Usuarios del dominio).

grupos: Los grupos secundarios en los que está incluido el usuario dentro de Active Directory.

Confirmación: Lo más importante es que esto demuestra que el servicio SSSD está funcionando correctamente, ya que el sistema Linux es capaz de resolver y reconocer la información de un usuario que vive en el servidor Windows.

### Documentación consultada
https://ubuntu.com/server/docs/how-to/sssd/with-active-directory/


## E8. Inicio de sesión con un usuario del dominio

Intenta iniciar sesión en el sistema Linux utilizando un usuario del dominio.

**Ejecuta:**

```bash
su - usuario1@empresa.local
```

![alt text](/images6/image-12.png)

- ¿Cómo funciona la autenticación contra el dominio?

Incluye capturas del proceso.

### Explicación
Para que Linux se entienda con el Active Directory, básicamente se usa el SSSD como puente. Cuando metemos el usuario, como Linux no lo tiene en sus archivos locales, le pide al SSSD que lo busque. Este se conecta al servidor de Windows por LDAP para ver si el usuario existe y qué grupos tiene asignados.

Para el tema de la contraseña y que todo sea seguro, entra Kerberos, que comprueba que la clave es correcta y nos da un ticket para poder pasar. El último paso es que el SSSD traduce los SIDs de Windows a números UID/GID, porque si no, Linux no sabría qué permisos darnos. Al final, el módulo PAM es el que nos abre la sesión y nos deja entrar al sistema con nuestra cuenta de red.

### Documentación consultada
https://ubuntu.com/server/docs/how-to/sssd/with-active-directory/



## E9. Creación automática del directorio personal

Comprueba que el sistema crea automáticamente el directorio personal del usuario del dominio.

**Ejecuta:**

```bash
ls /home
```

![alt text](/images6/image-13.png)

> NOTA: No se me ha creado el directorio personal.

- ¿Por qué se crea este directorio al iniciar sesión?

Incluye capturas.

### Explicación

Explica por qué se genera el home del usuario de dominio.

### Documentación consultada

Indica aquí las fuentes utilizadas.

---

## E10. Verificación en Active Directory

Desde el servidor abre **Active Directory Users and Computers**.

Comprueba que el equipo Linux aparece registrado en el dominio.

![alt text](/images6/image-14.png)

> Aquí se puede ver el equipo Linux registrado dentro del dominio con su nombre cli-linux01.


### Explicación
Los equipos Linux aparecen en Active Directory como equipos normales del dominio, con su nombre, por ejemplo cli-linux01.

Se muestran en la carpeta de “Computers” o en la unidad organizativa donde estén asignados. Desde ahí se pueden ver como cualquier otro ordenador de la red.
### Documentación consultada
Practicas anteriores.


## E11. Comprobación final de la integración

Comprueba que:

- el equipo Linux pertenece al dominio 

    ![alt text](/images6/image-15.png)

- el sistema reconoce usuarios del dominio  

    ![alt text](/images6/image-16.png)

- los usuarios pueden autenticarse desde Linux  

    ![alt text](/images6/image-17.png)

- se crean directorios personales al iniciar sesión

    No se me ha creado.

### Explicación
La integración funciona correctamente porque el equipo Linux está unido al dominio, puede usar usuarios del servidor, permite iniciar sesión con ellos y crea automáticamente las carpetas personales aunque a mi no se me ha creado.

### Documentación consultada
https://learn.microsoft.com/es-es/sql/linux/sql-server-linux-active-directory-join-domain?view=sql-server-ver17&tabs=rhel
