## READ FIRST 
- โน๊ตนี้เป็นโน๊ตบันทึก lab operating system 
- Ubunto Server 22.04

## คำสั่งพื้นฐาน
- update software
```shell
sudo apt update
sudo apt list --upgradable
```

- shut down and reboot 
```shell
sudo shutdown -h now
sudo reboot now
```

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
- check folder
```shell
ls -i
ls -ia
ls -l
ls -la
```
- check services 
```shell
ps -a | grep servicename
```
- show service status (+ หมายถึง service กำลัง run อยู่ , - หมายถึง service กำลัง stop อยู่)
```shell
serivce --status-all
```
- check specific service
```shell
sudo service apache2 status
```
- restart service 
```shell
sudo service apache2 restart
```
- check firewall (ดู firewall ที่เปิดอยู่)
```shell
sudo ufw status
sudo ufw status verbose
```
- open port (22 หมายเลข port ที่จะเปิด)
```shell
sudo allow port 22
```

## CONFIG IP
- ref : https://www.linuxtechi.com/static-ip-address-on-ubuntu-server/
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
- ref : https://linuxhint.com/install-enable-openssh-ubuntu-22-04/
1. install
```shell
sudo apt install openssh-server
```
2.open ssh everytime boots os (--now หมายถึงให้ start ตอนเปิดเครื่องทุกครั้งอย่างแน่นอน)
```shell
sudo systemctl enable --now ssh
```
3. check service
```shell
sudo service ssh status
```
## FTP
- ref 1 : https://linuxhint.com/ubuntu-ftp-22-04-server-configuration/
- ref 2 : https://itslinuxfoss.com/how-to-install-an-ftp-server-on-ubuntu-22-04/
## COCKPIT
- REF : https://www.techrepublic.com/article/install-cockpit-ubuntu-better-server/
## MYSQL
- REF : https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04
1. install
```shell
sudo apt install mysql-server
```
2. start services
```shell
sudo systemctl start mysql.service
```
3. config
- add secure to mysql
```shell
sudo mysql_secure_installation
```
4. create user
- access to mysql
```shell
sudo mysql
sudo mysql -u root -p
```
- create user 
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
- or create user on mysql with encrypt password 
```sql
CREATE USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
- privilege
```sql
GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```

## PHPMYADMIN
- ref : https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04
- isntall
```shell
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
- open php extention
```shell
sudo phpenmod mbstring
```
- restart apache2
```shell
sudo service apache2 restart
```
- check user account in mysql
```shell 
sudo mysql 
sudo mysql -u root -p 
```
```sql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
- change root user authen `auth_socket` to `caching_sha2_password`
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
```
- go to phpmyadmin `https://your_domain_or_IP/phpmyadmin`
- if you aready installed and php file not run try this
```shell
sudu apt install php
```
 

## OPENCART

## SAMBA

