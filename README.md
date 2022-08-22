# ZABBIX 6.2 - Install-Guide-Ubuntu-20.04

### 1. Install Zabbix repository

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

### 2. Install Zabbix server, frontend, agent

