###### Descargar el repositorio oficial de Zabbix de la siguiente ruta: https://github.com/gadiel379/raspberry-zabbix5.2/blob/main/zabbix-release_5.2-1%2Bdebian10_all.deb
 
 # PARA LA CONFIGURACION ES NECESARIO INSTALAR NANO
  * sudo apt install nano
 
>  comandos basicos
> Ctrl+w buscar
> Ctrl+x salir
  
 
# 1 INSTALAR REPOSITORIO DE Zabbix
* wget https://github.com/gadiel379/raspberry-zabbix5.2/blob/main/zabbix-release_5.2-1%2Bdebian10_all.deb
* dpkg -i zabbix-release_5.2-1+debian10_all.deb
* sudo apt update
 
 
# 2 INSTALAR SERVIDOR ZABBIX, FRONTED, AGENTE
* sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent


# 3 INSTALAR EL SERVIDOR DE BASE DE DATOS MARIADB
* sudo apt install mariadb-server
* sudo mysql_secure_installation

 Le pedira actualaizar la contraseña de root pulse Y escriba la nueva contraseña
 En los pasos siguientes solo ponga: Y
  
  
# 4 CREE EL USUARIO Y LA BASE DE DATOS DE ZABBIX
* sudo mysql -u root -p

CREE UN USUARIO Y UNA BASE DE DATOS DE ZABBIX COMO SE MUESTRA Y OTORGUE LOS PRIVILEGIOS
AL USUARIO DE LA SIGUIENTE MANERA:

* CREATE DATABASE zabbix_db character set utf8 collate utf8_bin;
* CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'zabbix';
* GRANT ALL PRIVILEGES ON zabbix_db.* TO 'zabbix'@'localhost' WITH GRANT OPTION;
* FLUSH PRIVILEGES;
* quit;


# 5 INSTALAR Y CONFIGURA APACHE
 * sudo apt update
 * sudo apt install  apache2 
 * sudo systemctl start apache2
 * sudo systemctl enable apache2


# 6 VERIFICA LA VERSION INSTALADA DE PHP
 * php -v  

DIRIJASE AL DIRECTORIO DE CONFIGURACIÓN DE PHP Y EDITE EL /etc/php/7.4/apache2/php.ini 
DE ACUERDO A LA VERCIÓN ES LA RUTA DE PHP. (7.3) O (7.4)

* sudo nano /etc/php/7.3/apache2/php.ini   
 
******************************************
* DATOS A MEDIFICAR OPCIONALES:
* memory_limit 256M
* upload_max_filesize 16M
* post_max_size 16M
* max_execution_time 300
* max_input_time 300
* max_input_vars 10000

* DATOS A MODIFICAR OBLIGATORIO, DESCOMENTAR:
* date.timezone="America/Merida"

* comandos basicos de nano
* Ctrl+x salir
* pedira guardar los datos, damos enter.
*******************************************

* SE RECOMIENDA REINICIAR EL SERVICIO DE PHP
* sudo systemctl restart apache2


# 7 ACTUALIZAMOS POR RECOMENDACION
* sudo apt update


# 8 CONFIGURAMOS SERVIDOR ZABBIX, EL ARCHIVO DE CONFIGURACION SE ENCUENTRA EN /ETC/ZABBIX/ZABBIX_SERVER.CONF
* sudo nano /etc/zabbix/zabbix_server.conf
 
*********************************************************************************************
* LOS DATOS A CAMBIAR SON LOS SIGUIENTES, DE ACUIERDO A LA BASE DE DATOS Y USUARIO CREADOS:
* DBHost=localhost
* DBName=zabbix_db
* DBUser=zabbix

* DESCOMENTAR PARA AGREGAR LA CONTRASEÑA DE LA BASE DE DATOS:
* DBPassword=zabbix

* comandos basicos de nano
* Ctrl+x salir
* pedira guardar los datos, damos enter.
*********************************************************************************************


# 9 CARGAMOS EL ESQUEMA PREDETERMINADO DE LA BASE DE DATOS DE ZABBIX
* zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix_db

EL PROCESO TARDARA UNOS MINUTOS.

# 10 INICIE LOS PROCESOS DE SERVIDOR Y AGENTE DE ZABBIX, INICIE LOS PROCESOS  DEL SERVIDOR  Y AGENTE ZABBIX.
* sudo systemctl restart zabbix-server zabbix-agent apache2
* sudo systemctl enable zabbix-server
* sudo systemctl status zabbix-server

Ctrl+c para terminar proceso


# 11 REINICIE EL SERVICIO DE APACHE2 Y VERIFIQUE EL ESTATUS
* sudo systemctl restart apache2
* sudo systemctl status apache2

Ctrl+c para terminar proceso


# 12 PARA SABER LA DIRECCIÓN DEL EQUIPO ESCRIBA
* IFCONFIG

#### DIRIGETE A LA SIGUIENTE RUTA PARA CONTINUAR CON LA INSTALACION DEL SERVIDOR ZABBIX.
http: // servidor-ip / zabbix
