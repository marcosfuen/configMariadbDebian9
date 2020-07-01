# Configuracion de Maria DB en Debian 9

_Pasos a seguir para configurar un servidor de mariadb en debian 9_

## Comenzando 🚀

_Estas instrucciones te permitirán configurar un servidor de bd con mariadb._

### Pre-requisitos 📋

_mariadb_

### Instalación 🔧

_Instalación de MariaDB_

**Primer Paso**

```
root@server:~# apt install mariadb-server mariadb-client
```
**Segundo Paso**

_Terminada la instalación, se debe ejecutar el comando mysql_secure_installation, que hace una serie de verificaciones e cambios en la configuración para garantizar la seguridad del servidor mysql._

```
root@server:~# mysql_secure_installation
```

_Salida que muestra_

```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!
 
In order to log into MariaDB to secure it, we`ll need the current
password for the root user.  If you`ve just installed MariaDB, and
you haven`t set the root password yet, the password will be blank,
so you should just press enter here.
 
Enter current password for root (enter for none):
OK, successfully used password, moving on...
 
Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.
 
Set root password? [Y/n] n
 ... skipping.
 
By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.
 
Remove anonymous users? [Y/n]
 ... Success!
 
Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.
 
Disallow root login remotely? [Y/n]
 ... Success!
 
By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.
 
Remove test database and access to it? [Y/n]
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!
 
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.
 
Reload privilege tables now? [Y/n]
 ... Success!
 
Cleaning up...
 
All done!  If you`ve completed all of the above steps, your MariaDB
installation should now be secure.
 
Thanks for using MariaDB!
root@server:~#
```
### Configuración ⚙️

_Permitir conexiones a nuestro servidor de bd_

**Paso 1**

_Esitar el archivo_

```
/etc/mysql/mariadb.conf.d/50-server.cnf, comentar la línea skip-external-locking y modificar la línea bind-address = 127.0.0.1 a bind-address = 0.0.0.0
```
**Paso 2**

_Reiniciamos el servicio Mariadb_

```
service mariadb restart

```
**Paso 3**

_Damos permiso al usuario para que se conecte desde cualquier equipo de la red o Internet._

_Creamos la bd_

```
CREATE DATABASE android;
```
_Damos los permisos al usuario que se conecte a la bd_

```
GRANT ALL PRIVILEGES ON android.* TO 'username'@'10.20.1.92' IDENTIFIED BY 'password' WITH GRANT OPTION;

```
_Dar todos los permisos a un usuario en mariadb_

```
GRANT ALL PRIVILEGES ON *.* TO username@'%' IDENTIFIED BY 'password';

```
_Permitir a un usuario conectarse a una base dato en particular desde una ip espesifica en este caso bd android e ip 192.168.0.20_

```
GRANT ALL PRIVILEGES ON android.* TO 'username'@'192.168.0.20' IDENTIFIED BY 'contraseña' WITH GRANT OPTION; 
```
_Después de dar los provilegios ejecuta este otro comando_

```
FLUSH PRIVILEGES;
```

**Firewall**

## Configuración ⚙️

_Instalación de ufw_

```
root@server:~# apt install ufw
```

_permiso a todas la ip entrantes_

```
root@server:~# ufw allow 3306/tcp
```
_exclusivamente a una o varias ip._

```
root@server:~# ufw allow from 192.168.1.2 to any port 3306
```

## Licencia 📄

Este proyecto está bajo la Licencia (GPL3) - mira el archivo [LICENSE.md](LICENSE) para detalles o el enlace actualizado [GPL_V3](https://www.gnu.org/licenses/gpl-3.0.html)

