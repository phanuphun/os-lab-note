# อ่านก่อน
โน๊ตนี้เป็นการจดบันทึกของวิชา Operating System Labotory โดย os ฝั่ง server ที่ใช้จะเป็น Ubuntu server 22.04 และฝั่ง client ที่ใช้จะเป็น ubuntu mate
## ติดตั้ง Wifi adapter สำหรับ Client
wifi adapter ที่ใช้จะเป็น RT8188FTV [อ่านวิธีติดตั้งได้ที่นี่](https://github.com/1999AZZAR/use-RTL8188FTV-on-linux)
## คำสั่ง Linux พื้นฐาน
- อัพเดต software `sudo apt update` จากนั้นก็ `sudo apt list --upgradable`
- ปิดเครื่องและรีสตาร์ท `sudo shutdown -h now` และ `sudo reboot now`
- ตรวจสอบ interface สำหรับ network `ip a` หรือ `ip a s dev enp1s0`
- รีสตาร์ท network `sudo ip link set enp1s0 down` และ `sudo ip link set enp1s0 up`
- ตรวจสอบ folder `ls -i` , `ls -ia` , `ls -l` , `ls -la`
- ตรวจสอบ services ในเครื่อง  `ps -a | grep ชื่อservice`
- ตรวจสอบสถานะ service `serivce --status-all` (+ หมายถึง service กำลัง run อยู่ , - หมายถึง service กำลัง stop อยู่)
- ดูสถานะ service นั้นๆ  `sudo service apache2 status`
- รีสตาร์ท service `sudo service apache2 restart`
- ดู firewall ที่เปิดอยู่ `sudo ufw status` หรือ `sudo ufw status verbose`
- เปิด port `sudo allow port 22` (22 หมายเลข port ที่จะเปิด)

## ตั้ง STATIC IP  
ใน lab จะตั้งค่า ip เป็น static และอยู่ในวงแลนเดียวกันกับเครื่องอื่นๆ [อ้างอิง](https://www.linuxtechi.com/static-ip-address-on-ubuntu-server/)
1. ไปที่ `/etc/netplan/` หาไฟล์  `00-installer-config.yaml` (ชื่อไฟล์อาจเปลี่ยนไปตามการกำหนดค่า server)
2. เข้าไปตาม path `cd /etc/netplan`
3. ตรวจดูชื่อไฟล์ `ls -l` ว่ามี  `00-installer-config.yaml` ไหม
4. แก้ไขไฟล์ `00-installer-config.yaml` ด้วยคำสั่ง `nano 00-installer-config.yaml` แล้วคัดลอกไปวาง (ตั้งค่าตาม lab)
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
## ติดตั้ง APACHE2
web server สำหรับการแสดงผลหน้าเว็บ [อ่านวิธีการติดตั้ง](https://linuxhint.com/install-enable-openssh-ubuntu-22-04/)
## ติดตั้ง SSH
ssh ใช้สำหรับการ Remote มาจากเครื่องอื่นหรือที่จะ remote มาจากเครื่อง client [อ้างอิง](https://linuxhint.com/install-enable-openssh-ubuntu-22-04/)
- ติดตั้ง `sudo apt install openssh-server`
- ให้เปิด ssh ทุกครั้งที่ boot ระบบด้วย `sudo systemctl enable --now ssh` (--now หมายถึงให้ start ตอนเปิดเครื่องทุกครั้งอย่างแน่นอน)
- ตรวจสอบสถานะ `sudo service ssh status`
## ติดตั้ง FTP
ใช้สำหรับการโอนย้ายไฟล์ต่างๆ [อ้างอิง 1](https://linuxhint.com/ubuntu-ftp-22-04-server-configuration/) และ [อ้างอิง 2](https://itslinuxfoss.com/how-to-install-an-ftp-server-on-ubuntu-22-04/)
## ติดตั้ง COCKPIT
ใช้สำหรับในการจัดการระบบด้วย web ui ง่ายต่อการใช้งาน [อ่านวิธีการติดตั้ง](https://www.techrepublic.com/article/install-cockpit-ubuntu-better-server/)
## ติดตั้ง MYSQL
สำหรับจัดการฐานข้อมูลต่างๆที่จำนำมาใช้กับ phpmyadmin และ opencart [อ้างอิง](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)
- ติดตั้ง `sudo apt install mysql-server`
- เปิด service `sudo systemctl start mysql.service`
- เพิ่มความปลอดภัยให้กับ mysql `sudo mysql_secure_installation`
- สร้าง user สำหรับ login โดยเข้าไปที่ mysql ก่อนด้วย `sudo mysql` หรือถ้าตั้งค่าการตรวจสอบ password ให้กับ root ตอนลงด้วยให้ใช้ `sudo mysql -u root -p` แล้วใส่ password ที่เราตั้งเอาไว้ 
- access to mysql
- เมื่อเข้ามาใน mysql แล้วใช้คำสั่ง mysql สร้าง `CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';` หรือใช้ `CREATE USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';` เพื่อเพิ่มการเข้ารหัสสำหรับ password
- กำหนดสิทธิ์ให้กับ user `GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;` และ `FLUSH PRIVILEGES;` เมื่อเสร็จหมดแล้วใช้ `exit` ในการออกจาก mysql
## ติดตั้ง PHPMYADMIN
web ui สำหรับการจัดการฐานข้อมูลซึ่งง่ายต่อการใช้งาน [อ้างอิง](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04)
- ติดตั้ง `sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl`
- เปิดใช้งาน extention ของ php `sudo phpenmod mbstring`
- restart apache2 `sudo service apache2 restart`
- ดู user account ใน mysql `sudo mysql ` หรือ `sudo mysql -u root -p ` เมื่อเข้ามาใน mysql ได้แล้วใช้คำสั่ง `SELECT user,authentication_string,plugin,host FROM mysql.user;` เพื่อดูรายชื่อ user ทั้งหมด
- เปลี่ยนการตรวจสอบความปลอดภัยของ root จาก `auth_socket` to `caching_sha2_password` ด้วยคำสั่ง `ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';`
- ไปที่ phpmyadmin ด้วย `https://your_domain_or_IP/phpmyadmin` เป็นการติดตั้งเสร็จสิ้น
- *** กรณีที่ phpmyadmin ไม่แสดงลองใช้คำสั่ง `sudu apt install php`
## การติดตั้ง OPENCART
เป็น web สำหรับขายของโดยที่เราไม่ต้องเขียนเว็บเองจำเป็นต้องมี mysql , phpmyadmin ก่อน [อ่านวิธีการติดตั้ง](https://www.linuxtuto.com/how-to-install-opencart-on-ubuntu-22-04/#)
- การตั้งค่าไฟล์ virturehost ของ opencart
```shell
<VirtualHost *:8083>
    ServerAdmin admin@192.168.1.206
    DocumentRoot /var/www/html/
    ServerName 192.168.1.206
    ServerAlias www.192.168.1.206

    Alias /opencart "/var/www/html/opencart/upload/"

    <Directory /var/www/html/opencart/upload/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/apache2/192.168.1.206-error_log
    CustomLog /var/log/apache2/192.168.1.206-access_log common
</VirtualHost>
```
## ติดตั้ง SAMBA
เป็น service สำหรับการแชร์ไฟล์ให้กันคนอื่นๆ [วิธีการติดตั้ง](https://phoenixnap.com/kb/ubuntu-samba) และ [การเข้าถึง folder](https://linuxsimply.com/how-to-access-samba-share-from-windows/)
- ตัวอย่างการเข้าถึง folder ที่แชร์ `\\192.168.1.206\sharingfolder`
- ตรวจสอบ user account ที่สร้างใน samba `sudo pdbefit -L -v`
- ตรวจสอบ config ที่เราพึ่ง config ไป `testparm`
- ติดตั้ง acl สำหรับการติดตั้งบาง package ในตัวอย่าง `sudo apt-get install acl`


