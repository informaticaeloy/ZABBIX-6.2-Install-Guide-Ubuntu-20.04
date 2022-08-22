# ZABBIX 6.2 - Install-Guide-Ubuntu-20.04

### 1. Instalar el repositorio Zabbix

> En la URL https://www.zabbix.com/download tenemos un configurador de entorno en el que nos muestra los comandos a usar dependiendo de la elección de las opciones de que dispongamos, en nuestro caso: Zabbix 6.2, Ubuntu 20.04 (Focal), MySql y Apache Server

> Descargamos el paquete .deb

 ```shell 
 wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-1+ubuntu20.04_all.deb
 ```
 
![image](https://user-images.githubusercontent.com/20743678/185880032-20d84c7e-1fc2-4b3c-a2fc-25246b766a6b.png)

> Instalamos el paquete
 
```shell
 dpkg -i zabbix-release_6.2-1+ubuntu20.04_all.deb
  ```
  
![image](https://user-images.githubusercontent.com/20743678/185880223-a0dfdf5c-6e5b-4b09-a664-fb926abe15fd.png)

> Actualizamos los repositorios definidos en el archivo /etc/apt/sources.list

 ```shell
 sudo apt-get update
  ```
![image](https://user-images.githubusercontent.com/20743678/185880793-1b3e1d89-45df-4305-be0e-0b31be0e32e1.png)

### 2. Instalar Zabbix server, el frontend y el agente

```shell
sudo apt-get install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

![image](https://user-images.githubusercontent.com/20743678/185893740-82c5e0cb-7a6e-4cb1-aa49-a60fdeeb9da9.png)

...

![image](https://user-images.githubusercontent.com/20743678/185896267-c13c22cf-38c7-430e-add6-0304650dc8a7.png)

### 3. Instalar MySQL server

> Instalamos el paquete

```shell
apt-get install mysql-server
```

![image](https://user-images.githubusercontent.com/20743678/185899040-86690bd1-cbaa-4d07-86db-51182256d534.png)

> Configuramos el acceso para el usuario ROOT

```shell
sudo mysql
```

```shell
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'password_nuevo';
```

```shell
exit
```

![image](https://user-images.githubusercontent.com/20743678/185900620-9defc4f1-fdd4-4917-b21e-2b2cbded119b.png)

> Configuramos la seguridad de MySQL

```shell
sudo mysql_secure_installation
```

![image](https://user-images.githubusercontent.com/20743678/185901446-a46e4c96-64b7-4aec-aa69-a57821b9794a.png)

...

![image](https://user-images.githubusercontent.com/20743678/185901679-1e906ac2-0cfe-4a1a-be88-cb457b20f257.png)

### 4. Crear la base de datos inicial

> Nos aseguramos que MySQL está UP y corriendo

```shell
sudo systemctl status mysql
```

![image](https://user-images.githubusercontent.com/20743678/185902113-1cc3983c-6672-4c58-8917-e1784a8a6c73.png)

> Entramos a la shell de MySQL

```shell
mysql -uroot -p
```

![image](https://user-images.githubusercontent.com/20743678/185902506-bbbef112-bc1a-4de2-b7a1-0bd4fce4d4f9.png)

> Creamos una base de datos que se llama 'zabbix'

```shell
create database zabbix character set utf8mb4 collate utf8mb4_bin;
```

> Creamos un usuario dentro de MySQL que se llama 'zabbix@localhost'

```shell
create user zabbix@localhost identified by 'password';
```

> Asignamos todos los permisos al usuario 'zabbix@localhost' en la base de datos 'zabbix'

```shell
grant all privileges on zabbix.* to zabbix@localhost;
```

> Definimos la variable 'log_bin_trust_function_creators' con valor '1' y ámbito global, para que no sea necesario disponer de privilegios de root para la creación de funciones

```shell
SET GLOBAL log_bin_trust_function_creators = 1;
```

> Salimos de MySQL

```shell
quit;
```

![image](https://user-images.githubusercontent.com/20743678/185903268-fbc6e151-ab85-433b-a272-964c3dc43af4.png)

> Si quieremos posteriormente cambiar la contraseña del usuario, entramos en la shell de MySQL con credenciales de root:

```shell
mysql -u root -p 
```

> y en la Shell de MySQL ejecutamos lo siguiente:

```shell
ALTER USER 'zabbix'@'localhost' IDENTIFIED BY 'NuevaContraseña';
```

> Cuando hicimos la instalación inicial de Zabbix, se creó una carpeta en '/usr/share/doc/zabbix-sql-scripts/mysql' donde hay unfichero con la estructura inicial de la base de datos, llamado 'server.sql.gz'. Lo descomprimimos y lo metemos a nuestra base de datos de MySQL:

```shell
zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
```

![image](https://user-images.githubusercontent.com/20743678/185907347-029922ae-8143-4a5e-b644-e76d220a7234.png)

> Una vez importado el schema, volvemos a deshabilitar la variable 'log_bin_trust_function_creators' asignándole valor '0' para incrementar la seguridad:

```shell
mysql -uroot -p
```

> Dentro de la shell de MySQL:

```shell
SET GLOBAL log_bin_trust_function_creators = 0;
```

```shell
quit;
```

### 5. Configurar la base de datos para el servidor de Zabbix

> Editamos el fichero '/etc/zabbix/zabbix_server.conf'

```shell
sudo nano /etc/zabbix/zabbix_server.conf
```

> Y sobre la línea 129 aproximadamente, definimos el password que asignamos al usuario 'zabbix' en MySQL:

```shell
DBPassword=password_del_usuario_zabbix
```

![image](https://user-images.githubusercontent.com/20743678/185908143-538df62c-c0f0-4e23-a57f-cd41684416c0.png)

### 6. Iniciamos el servidor Zabbix y lo configuramos con autoarranque

> Reiniciamos el servidor Zabbix y el Apache

```shell
systemctl restart zabbix-server zabbix-agent apache2
```

> Configuramos el autoarranque con el inicio del sistema

```shell
systemctl enable zabbix-server zabbix-agent apache2
```

![image](https://user-images.githubusercontent.com/20743678/185908861-8cb23dd7-1d4a-4c80-8b1f-2e18e7642e5f.png)

> Comprobamos si todo está levantado y corriendo:

```shell
sudo systemctl status apache2
```

![image](https://user-images.githubusercontent.com/20743678/185909070-23b372bf-71c3-43f2-8db4-7aedb03a6d82.png)


```shell
sudo systemctl status zabbix-server
```

![image](https://user-images.githubusercontent.com/20743678/185909246-2aca5f47-23bc-4a5c-b597-8f22197e4d10.png)

```shell
sudo systemctl status zabbix-agent
```

![image](https://user-images.githubusercontent.com/20743678/185909345-b900d71c-72b5-422e-87c3-6d310d2ab9ea.png)


### 6. Configurar el frontend de Zabbix

> Accedemos a la URL de nuestro servidor:

```shell
http://192.168.46.214/zabbix
```

![image](https://user-images.githubusercontent.com/20743678/185912693-bf9d70b4-0067-4a1f-8f9b-3664f99ea7ba.png)




