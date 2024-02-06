## READ FIRST 
Ubunto Server 22.04
## LINUX BASIC COMMAND
1.VERY BASIC
- update software
```bat
sudo apt update
sudo apt list --upgradable
```

- shut down and reboot 
```shell
sudo shutdown -h now
sudo reboot now
```

2.NETWORK
- check interface
```shell
ip a
```
```shell
ip a s dev enp1s0 
```
- restart network
```shell
sudo ip link set enp1s0 down
sudo ip link set enp1s0 up
```
 
3.DIRECTORY PATH
```shell
ls -i
ls -ia
ls -l
ls -la
```

4.SERVICE 
4.1 FIND SERVICE  (ape คือชื่อ service ที่จะหา)
```shell
ps -a | grep ape 
```
4.2 SHOW ALL SERVICE STATUS (+ หมายถึง service กำลัง run อยู่ , - หมายถึง service กำลัง stop อยู่)
```shell
serivce --status-all
```
4.3 CHECK SPECIFIC SERVICE STATUS
```shell
sudo service apache2 status
```
4.4 RESTART SERVICE
```shell
sudo service apache2 restart
```

5.FIREWALL
5.1 CHECK FIREWALL (ดู firewall ที่เปิดอยู่)

```shell
sudo ufw status
sudo ufw status verbose
```
5.2 OPEN PORT (22 หมายเลข port ที่จะเปิด)
```shell
sudo allow port 22
```
## CONFIG SERVER FIRST
REF : https://www.linuxtechi.com/static-ip-address-on-ubuntu-server/
1. ไปที่ `/etc/netplan/` หาไฟล์  `00-installer-config.yaml` (ชื่อไฟล์อาจเปลี่ยนไปตามการกำหนดค่า server)
```shell
cd /etc/netplan/
ls -l
```
2. เข้าถึงไฟล์ `00-installer-config.yaml`
```shell
nano 00-installer-config.yaml
```
```
network:
    version:2
    renderer: networkd
    ethernets:
        enp0s3:
            addresses:
                - 192.168.1.45/75
            nameserver:
                addresses: [8.8.8.8, 8.8.4.4] 
            routes:
                - to: default
                  via: 192.168.1.1
```


## APACHE2

## SSH
- สำหรับการ Remote มาจากเครื่องอื่น
- REF : https://linuxhint.com/install-enable-openssh-ubuntu-22-04/
1. INSTALL
```shell
sudo apt install openssh-server
```
2. ENABLE OPEN SSH (--now หมายถึงให้ start ตอนเปิดเครื่องทุกครั้งอย่างแน่นอน)
```shell
sudo systemctl enable --now ssh
```
3. CHECK STATUS
```shell
sudo service ssh status
```

## FTP
- REF 1 : https://linuxhint.com/ubuntu-ftp-22-04-server-configuration/
- REF 2 : https://itslinuxfoss.com/how-to-install-an-ftp-server-on-ubuntu-22-04/

## COCKPIT
- REF : https://www.techrepublic.com/article/install-cockpit-ubuntu-better-server/
## MYSQL
- REF : https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04
1. INSTALL
```shell
sudo apt install mysql-server
```
2. START MYSQL
```shell
sudo systemctl start mysql.service
```
3. CONFIG
3.1 ADD SECURITY TO MYSQL
```shell
sudo mysql_secure_installation
```
4. CREATE USER
4.1 ACCESS TO MYSQL
```shell
sudo mysql
sudo mysql -u root -p
```
4.2 CREATE USER ON MY SQL
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
OR 4.2 CREATE USER ON MY SQL WITH ENCRYPT PASSWORD 
```sql
CREATE USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
4.3 PRIVILEGE
```sql
GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```

## PHPMYADMIN
- REF : https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04
1. INSTALL
```shell
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
2. OPEN PHP EXTENTION
```shell
sudo phpenmod mbstring
```
3. RESTART APACHE 2
```shell
sudo service apache2 restart
```
4. CHECK USER ACCOUNT IN MYSQL
```shell 
sudo mysql 
sudo mysql -u root -p 
```
```sql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
5. CHECNG ROOT USER AUTHENTICATION `auth_socket` TO `caching_sha2_password`
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
```
6. GO TO PHPMYADMIN `https://your_domain_or_IP/phpmyadmin`

- FIX 1 : IF YOU AREADY INSTALLED AND PHP FILE NOT RUN TRY THIS
```shell
sudu apt install php
```
- FIX 2 :

## OPENCART

## SAMBA

