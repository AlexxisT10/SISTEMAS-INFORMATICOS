# PRÁCTICA 1 — ADMINISTRACIÓN DE SISTEMAS (UD4)

---

# 1. Gestión avanzada de usuarios y grupos

## 1.1 Crear usuario dev01 sin opciones

Crea un usuario llamado **dev01** utilizando `useradd` sin opciones adicionales.

```bash
sudo useradd dev01
```
![alt text](./images/image.png)

### Comprobaciones

Comprobar si se ha creado su directorio personal:

```bash
ls /home
```

> Por defecto **no se crea el directorio personal**.




=======
![alt text](./images/image-1.png)

---

Comprobar su entrada en `/etc/passwd`.

```bash
cat /etc/passwd | grep dev01
```


=======
![alt text](./images/image-2.png)

---

Comprobar su grupo por defecto en `/etc/group`.

```bash
cat /etc/group | grep dev01
```

![alt text](./images/image-3.png)

---

## 1.2 Mostrar configuración por defecto de useradd

Mostrar la configuración por defecto del sistema para la creación de usuarios.

```bash
useradd -D
```

![alt text](./images/image-4.png)

![alt text](./images/image-5.png)
---

## 1.3 Crear usuario dev02 con directorio personal

Crea un segundo usuario llamado **dev02** utilizando la opción para crear automáticamente su directorio personal.

```bash
sudo useradd -m dev02
```

Comprobar el directorio:

```bash
ls /home
```

![alt text](./images/image-6.png)

---

## 1.4 Comprobar contenido de /etc/skel

Mostrar el contenido del directorio `/etc/skel`.

```bash
ls /etc/skel
```

![alt text](./images/image-7.png)
---

## 1.5 Crear usuario dev03 con adduser

Crear un tercer usuario llamado **dev03** utilizando:

```bash
sudo adduser dev03
```

![alt text](./images/image-8.png)

### Comprobaciones

Entrada en `/etc/passwd`:

```bash
cat /etc/passwd | grep dev03
```

![alt text](./images/image-9.png)

---

Entrada en `/etc/shadow`:

```bash
sudo cat /etc/shadow | grep dev03
```

![alt text](./images/image-10.png)

---

Grupo asociado:

```bash
groups dev03
```

![alt text](./images/image-11.png)

---

## 1.6 Mostrar configuración de adduser

Mostrar el contenido del archivo `/etc/adduser.conf`.

```bash
cat /etc/adduser.conf
```

![alt text](./images/image-12.png)

---

## 1.7 Cambiar al usuario dev01

```bash
su dev01
```

Si no permite acceso, asignar contraseña:

```bash
sudo passwd dev01
```
> Despues de ejecutarlo, hacemos:  **su dev01**. 


![alt text](./images/image-13.png)
---

## 1.8 Crear grupo devs

```bash
sudo addgroup devs
```

![alt text](./images/image-14.png)
---

## 1.9 Añadir usuarios al grupo

Añadir los tres usuarios al grupo devs.

```bash
sudo usermod -aG devs dev01
sudo usermod -aG devs dev02
sudo usermod -aG devs dev03
```
![alt text](./images/image-15.png)

### Comprobación

Primera forma:

```bash
grep devs /etc/group
```
![alt text](./images/image-16.png)

Segunda forma:

```bash
groups dev01 dev02 dev03
```

![alt text](./images/image-17.png)

---

## 1.10 Trabajar con el usuario dev03

Cambiar de usuario:

```bash
su dev03
```

Crear directorio:

```bash
mkdir ~/grupo
```

![alt text](./images/image-18.png)

---

## 1.11 Crear archivo con miembros del grupo

Generar archivo con los usuarios del grupo devs.

```bash
getent group devs > ~/grupo/members.txt
```

![alt text](./images/image-19.png)

---

## 1.12 Mostrar permisos del archivo

```bash
ls -l ~/grupo/members.txt
```

![alt text](./images/image-20.png)

---

## 1.13 Modificar permisos

Solo el propietario puede modificar, el grupo puede leer y otros sin acceso.

```bash
chmod 640 ~/grupo/members.txt
```
![alt text](./images/image-21.png)

Comprobar:

```bash
ls -l ~/grupo/members.txt
```

![alt text](./images/image-22.png)

---

## 1.14 Bloquear usuario dev02

```bash
sudo usermod -L dev02
```

![alt text](./images/image-23.png)

> El sistema añade un `!` delante del hash de la contraseña en `/etc/shadow`.

![alt text](/images/image-24.png)

---

## 1.15 Desbloquear usuario

```bash
sudo usermod -U dev02
```

![alt text](./images/image-25.png)

---

## 1.16 Configurar caducidad de usuario

```bash
sudo chage -E 2026-09-01 dev03
```

![alt text](./images/image-26.png)

Comprobar configuración:

```bash
chage -l dev03
```

![alt text](./images/image-27.png)

---

## 1.17 Eliminar usuarios

Eliminar usuario:

```bash
sudo userdel dev01
```

![alt text](./images/image-28.png)

Comprobar si existe el directorio:

```bash
ls /home
```

![alt text](./images/image-29.png)

---

Eliminar usuario junto con su directorio personal:

```bash
sudo userdel -r dev02
```

![alt text](./images/image-30.png)

---

# 2. Seguridad y permisos

## 2.1 Crear estructura de directorios

Estructura:

```
/project
/project/code
/project/tests
```

Crear directorios:

```bash
sudo mkdir /project/code
sudo mkdir /project/tests
```

![alt text](./images/image-31.png)

---

## 2.2 Crear archivos de prueba

```bash
touch alexis_prueba.txt /project
touch alexis_prueba.txt /project/code
touch alexis_prueba.txt /project/tests

```

![alt text](./images/image-32.png)

---

## 2.3 Cambiar grupo propietario

```bash
sudo chown :devs /project/code
```

![alt text](./images/image-33.png)
---

## 2.4 Configurar permisos

Solo los miembros del grupo puedan crear y modificar archivos en /code.
```bash
sudo chmod 770 /project/code
```
![alt text](./images/image-34.png)

En /tests solo el propietario pueda modificar.
```bash
sudo chmod 744 /project/tests
```
![alt text](./images/image-35.png)

Otros usuarios no tengan permisos de escritura.
```bash
sudo chmod 775 project
```
![alt text](./images/image-36.png)
---

## 2.5 Activar bit SGID

```bash
sudo chmod g+s /project/code
```

![alt text](./images/image-37.png)

Comprobación:

![alt text](./images/image-38.png)


> El bit SGID hace que los archivos nuevos hereden el grupo del directorio.
![alt text](./images/image-39.png)

---

## 2.6 Mostrar permisos

Formato largo:

```bash
ls -l /project
```
![alt text](./images/image-40.png)

Formato numérico:

```bash
stat /project/code
```
![alt text](./images/image-41.png)
---

## 2.7 Modificar umask

```bash
umask 027
```
![alt text](./images/image-42.png)


Crear archivo de prueba:

```bash
touch prueba.txt
ls -l
```

![alt text](./images/image-43.png)

---

## 2.8 Cambiar propietario y grupo

```bash
sudo chown usuario:grupo archivo
```

![alt text](./images/image-44.png)

---

## 2.9 Acceso restringido

Intentar acceder con usuario sin permisos.

```bash
cd /project/tests
```

![alt text](./images/image-45.png)

---

# 3. Servicios y procesos

## 3.1 Listar servicios activos

```bash
systemctl list-units --type=service
```

![alt text](./images/image-46.png)

---

## 3.2 Servicios habilitados al inicio

```bash
systemctl list-unit-files --type=service
```

![alt text](./images/image-47.png)

---

## 3.3 Estado del servicio SSH

```bash
systemctl status ssh
```

![alt text](./images/image-48.png)

---

## 3.4 Habilitar y deshabilitar servicio

```bash
sudo systemctl enable servicio
sudo systemctl disable servicio
```

![alt text](./images/image-49.png)

---

## 3.5 Detener e iniciar servicio

```bash
sudo systemctl stop servicio
sudo systemctl start servicio
```

![alt text](./images/image-50.png)

---

## 3.6 Procesos por CPU

```bash
ps aux --sort=-%cpu
```

![alt text](./images/image-51.png)

---

## 3.7 Procesos por memoria

```bash
ps aux --sort=-%mem|head
```

![alt text](./images/image-52.png)

---

## 3.8 Información de proceso por PID

```bash
ps -fp 1
```

![alt text](./images/image-53.png)

---

## 3.9 Finalizar proceso

```bash
sudo kill 4
```

![alt text](./images/image-54.png)

---

## 3.10 Ver logs del sistema

```bash
journalctl -u ssh
```

![alt text](./images/image-55.png)
