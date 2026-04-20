# UD5-P3. Implementación de un dominio Active Directory

## Escenario
En esta práctica se desplegará una infraestructura básica de dominio utilizando Active Directory.
Un servidor Windows actuará como controlador de dominio y un equipo Windows 11 se unirá al dominio como cliente.

Se crearán usuarios de dominio, se configurarán perfiles móviles y se modificará la política de contraseñas mediante GPO para observar el funcionamiento de las directivas de dominio.

Esta práctica permite comprender el funcionamiento básico de los dominios Windows y la administración centralizada de usuarios y recursos.

La actividad está relacionada con la gestión de recursos en red y las directivas de seguridad, aspectos incluidos en el RA6 del módulo. :contentReference[oaicite:0]

## E1. Configuración inicial del servidor

En el servidor:

**Cambia el nombre del equipo a:**  
**srv-ad01**

**Reinicia el sistema.**

**Comprueba el nombre del equipo ejecutando:**

```bash
hostname
```
![alt text](/images5/image.png)

- ¿Cuál es el nombre actual del equipo tras realizar el cambio?
    
    srv-ad01.

- ¿Qué papel tendrá este servidor dentro de la red?

    Este servidor tendrá el papel de Controlador de Dominio dentro de la red.


### Explicación
Primero se cambió el nombre del equipo desde la configuración del sistema, asignándole el nombre srv-ad01.

Después se reinició el servidor para aplicar los cambios.

Una vez reiniciado, se comprobó el nombre usando el comando:
```bash
hostname
```
Este servidor funcionará como Controlador de Dominio, encargado de administrar los usuarios y equipos dentro de la red.

### Documentación consultada
https://learn.microsoft.com/es-es/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-?utm_source=chatgpt.com


## E2. Instalación del rol Active Directory Domain Services

**Abre Server Manager.**

**Selecciona:**  
**Add Roles and Features**

**Instala el rol:**  
**Active Directory Domain Services**

![alt text](/images5/image-1.png)
>Selección del rol de AD DS

![alt text](/images5/image-2.png)
>Instalación correcta del rol AD DS

Durante el proceso se instalarán también las herramientas de administración necesarias.

- ¿Qué función cumple Active Directory Domain Services?

    Active Directory Domain Services permite crear un dominio y administrar usuarios y equipos dentro de una red.

    Permite:

    Autenticar usuarios

    Administrar permisos

    Organizar equipos

    Centralizar la administración

### Explicación

Active Directory Domain Services es un servicio que permite almacenar información sobre usuarios, equipos y recursos dentro de una red. Permite la autenticación centralizada y la administración de permisos, facilitando la gestión de grandes redes informáticas.

AD DS permite crear dominios, controlar el acceso a recursos y aplicar políticas de seguridad de forma centralizada.

### Documentación consultada
https://learn.microsoft.com/es-es/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-?utm_source=chatgpt.com


## E3. Promoción del servidor a controlador de dominio

Una vez instalado el rol:

**Selecciona:**  
**Promote this server to a domain controller**

![alt text](/images5/image-3.png)

**Selecciona la opción:**  
**Add a new forest**

![alt text](/images5/image-4.png)

**Nombre del dominio:**  
**empresa.local**

![alt text](/images5/image-5.png)

**Completa el asistente utilizando la configuración por defecto.**

**Define una contraseña para el modo de restauración (DSRM).**

**Finaliza el asistente y reinicia el servidor.**

![alt text](/images5/image-6.png)

- ¿Qué significa convertir un servidor en controlador de dominio?

    Convertir un servidor en controlador de dominio significa que el servidor pasa a gestionar el dominio y autenticar usuarios.

### Explicación

Convertir un servidor en controlador de dominio significa que dicho servidor será el encargado de gestionar la autenticación de usuarios y equipos dentro del dominio. El controlador de dominio almacena la base de datos de Active Directory y permite controlar el acceso a los recursos de la red.

También permite aplicar políticas de seguridad, administrar usuarios y controlar equipos desde un punto centralizado.

### Documentación consultada
https://learn.microsoft.com/en-us/%20%20%20%20%20%20windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-?utm_source=chatgpt.com


## E4. Verificación del dominio

Tras el reinicio:

**Inicia sesión con la cuenta:**  
**empresa\Administrator**

![alt text](/images5/image-7.png)

**Abre la herramienta:**  
**Active Directory Users and Computers**

**Comprueba que aparece el dominio:**  
**empresa.local**

![alt text](/images5/image-8.png)

- ¿Qué estructura básica aparece en el dominio?

    La estructura básica incluye:

    Builtin

    Computers

    Domain Controllers

    Users



### Explicación
Al crear un dominio en Active Directory se genera una estructura básica que incluye varios contenedores predeterminados. Entre ellos se encuentran:

Users: contiene las cuentas de usuario y grupos del dominio.

Computers: almacena los equipos que se unen al dominio.

Domain Controllers: contiene los servidores que actúan como controladores de dominio.

Esta estructura permite organizar los recursos y facilitar su administración dentro de la red.

### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-domain-services



## E5. Creación de usuarios de dominio

En **Active Directory Users and Computers** crea dos usuarios:

**usuario1**  
**usuario2**

![alt text](/images5/image-9.png)

> Usuario1 creado.

![alt text](/images5/image-10.png)

>Usuario2 creado.


**Asigna una contraseña inicial a cada uno.**

- ¿Cuál es la diferencia entre:  
  - usuario local  
  - usuario de dominio

    Usuario local

    Solo existe en un equipo, 
    no funciona en otros equipos, 
    administración individual

    Usuario de dominio

    Existe en Active Directory, 
    puede iniciar sesión en varios equipos, 
    administración centralizada

### Explicación
Un usuario local es una cuenta creada en un único equipo y solo permite iniciar sesión en ese equipo específico.

Un usuario de dominio es una cuenta almacenada en Active Directory que permite iniciar sesión en cualquier equipo que pertenezca al dominio. Este tipo de usuario facilita la administración centralizada y el acceso a recursos compartidos dentro de la red.

### Documentación consultada

https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/


## E6. Preparación de la carpeta de perfiles móviles

En el servidor:

**Crea la carpeta:**

```bash
C:\perfiles
```
![alt text](/images5/image-11.png)

**Comparte la carpeta con el nombre:**  
**perfiles**
![alt text](/images5/image-12.png)

![alt text](/images5/image-13.png)

- ¿Qué función tendrá esta carpeta dentro del dominio?

    La carpeta perfiles se utilizará para almacenar los perfiles móviles de los usuarios del dominio. Cada usuario tendrá su propia carpeta dentro de este recurso compartido. Esto permite que los usuarios mantengan su configuración y archivos personales independientemente del equipo desde el que inicien sesión.


### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/storage/file-server/file-server-resource-manager



## E7. Configuración de perfiles móviles

En **Active Directory Users and Computers:**

**Edita las propiedades de cada usuario.**

**En la pestaña Profile, configura la ruta del perfil:**

```bash
\\srv-ad01\perfiles\%username%
```
![alt text](/images5/image-14.png)

- ¿Qué es un perfil móvil?  

    Un perfil móvil es un perfil de usuario que se almacena en un servidor en lugar de guardarse únicamente en un equipo local. Cuando un usuario inicia sesión en un equipo del dominio, su perfil se descarga desde el servidor y cuando cierra sesión se vuelve a guardar en el servidor.

- ¿Qué ventajas ofrece en un entorno de dominio?

    Los perfiles móviles permiten que los usuarios puedan iniciar sesión desde diferentes equipos manteniendo sus archivos, configuraciones y preferencias. También facilitan la copia de seguridad y la administración centralizada de los perfiles de usuario.


### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/storage/folder-redirection/deploy-roaming-user-profiles



## E8. Unión del equipo Windows 11 al dominio

En el equipo cliente **cli01:**

**Abre la configuración del sistema.**

**Cambia el tipo de red para unir el equipo al dominio:**

**Dominio:**

```bash
empresa.local
```

![alt text](/images5/image-15.png)


**Cuando se solicite, introduce las credenciales de administrador del dominio.**

![alt text](/images5/image-17.png)

**Reinicia el equipo.**

- ¿Qué significa unir un equipo a un dominio?

    Unir un equipo a un dominio significa que dicho equipo pasará a formar parte de una red administrada por un controlador de dominio. A partir de ese momento, los usuarios del dominio podrán iniciar sesión en el equipo utilizando sus cuentas de dominio y se podrán aplicar políticas de seguridad centralizadas.

### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/join-computer-to-domain?tabs=cmd&pivots=windows-server-2025



## E9. Inicio de sesión con usuarios de dominio

En el cliente Windows 11:

**Inicia sesión con:**

```bash
empresa\usuario1
```
![alt text](/images5/image-18.png)

Tras iniciar sesión:

**Cierra la sesión y vuelve a iniciarla con:**

```bash
empresa\usuario2
```
![alt text](/images5/image-19.png)

**Comprueba que en el servidor se crean las carpetas de perfil dentro del recurso compartido.**

- ¿Cómo funcionan los perfiles móviles?

    Los perfiles móviles funcionan almacenando la información del usuario en el servidor. Cuando un usuario inicia sesión en un equipo del dominio, su perfil se descarga desde el servidor. Cuando cierra sesión, los cambios realizados se guardan nuevamente en el servidor.

    Esto permite que el usuario mantenga sus archivos y configuraciones independientemente del equipo que utilice.

### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/storage/folder-redirection/roaming-user-profiles-overview


## E10. Modificación de la política de contraseñas

En el servidor:

**Abre la herramienta:**  
**Group Policy Management**

**Localiza la política:**  
**Default Domain Policy**
![alt text](/images5/image-20.png)

**Edita la política y modifica alguno de los parámetros de contraseña, por ejemplo:**

- **longitud mínima de contraseña**  

![alt text](/images5/image-21.png)

- **complejidad de contraseña**
![alt text](/images5/image-22.png)

**Aplica los cambios.**

- ¿Qué son las Group Policy Objects (GPO)? 

    Las Group Policy Objects son configuraciones que permiten administrar de forma centralizada el comportamiento de usuarios y equipos dentro de un dominio. Permiten establecer reglas de seguridad, configuraciones del sistema y restricciones para todos los equipos del dominio.

    Las GPO facilitan la administración y aumentan la seguridad al aplicar configuraciones automáticamente en todos los equipos.

- ¿Qué función tienen en un dominio?

    Permiten:

    Configurar contraseñas, 
    definir seguridad, 
    controlar equipos, 
    Aplicar configuraciones automáticamente

    Las políticas se aplican a usuarios y equipos desde el servidor de dominio.


### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview



## E11. Verificación de la política aplicada

En el cliente ejecuta:

```bash
gpupdate /force
```
![alt text](/images5/image-23.png)

Después:

**Intenta cambiar la contraseña de uno de los usuarios para comprobar que se aplica la nueva política.**
![alt text](/images5/image-24.png)

- ¿Cómo se aplican las políticas de dominio en los equipos cliente?
    
    Las políticas de dominio se aplican automáticamente a los equipos cliente cuando se conectan al dominio. También pueden actualizarse manualmente mediante el comando gpupdate. Esto permite que los cambios realizados en el servidor se distribuyan a todos los equipos del dominio.


### Documentación consultada
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/gpupdate


## E12. Comprobación final del dominio

**Comprueba que:**

- el equipo **cli01** pertenece al dominio 

![alt text](/images5/image-25.png) 

- los usuarios pueden iniciar sesión en el dominio  

![alt text](/images5/image-26.png)
> Usuario1 iniciado correctamente al dominio. 

![alt text](/images5/image-27.png)
> Usuario2 iniciado correctamente al dominio.

- los perfiles móviles se almacenan en el servidor 

![alt text](/images5/image-28.png)

- la política de contraseñas se aplica correctamente

![alt text](/images5/image-29.png)

- ¿Cómo funciona el sistema de dominio implementado en conjunto?

    El dominio implementado permite la administración centralizada de usuarios, equipos y políticas de seguridad. El servidor actúa como controlador de dominio, almacenando la información de Active Directory y gestionando la autenticación.

    Los usuarios pueden iniciar sesión en cualquier equipo del dominio utilizando sus cuentas. Los perfiles móviles permiten mantener configuraciones y archivos personales en el servidor. Las políticas de grupo permiten aplicar configuraciones de seguridad y administración de manera automática en todos los equipos del dominio.

### Documentación consultada
Documentacion oficial de Microsoft:
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-?utm_source=chatgpt.com

https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/


