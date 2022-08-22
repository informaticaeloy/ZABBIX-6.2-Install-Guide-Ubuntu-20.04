# ZABBIX 6.2 - Install-Guide-Ubuntu-20.04

### 1. Install Zabbix repository

wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-1+ubuntu20.04_all.deb
![image](https://user-images.githubusercontent.com/20743678/185880032-20d84c7e-1fc2-4b3c-a2fc-25246b766a6b.png)

dpkg -i zabbix-release_6.2-1+ubuntu20.04_all.deb
![image](https://user-images.githubusercontent.com/20743678/185880223-a0dfdf5c-6e5b-4b09-a664-fb926abe15fd.png)

sudo apt-get update
![image](https://user-images.githubusercontent.com/20743678/185880510-f801a243-154e-4f1b-a749-9de1874d622d.png)

### 2. Install Zabbix server, frontend, agent

