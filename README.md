# Jarkom-Modul-3-E20
Nama Anggota :

1. Ahmad Hafiz Mahardika (5025201196)
2. Moch. Taslam Gustino P (5025211011)

# Lapres

## Topologi

![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/7b38c3e1-334f-4a0e-96ef-31443e2c38f6)

## Config

- Aura (DHCP Relay)
``` auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.216.0.0/16

auto eth1
iface eth1 inet static
	address 192.216.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.216.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.216.3.10
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.216.4.10
	netmask 255.255.255.0
```

- Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 192.216.1.2
	netmask 255.255.255.0
	gateway 192.216.1.1
```

- Heiter (DNS Server)
```
auto eth0
iface eth0 inet static
	address 192.216.1.3
	netmask 255.255.255.0
	gateway 192.216.1.1
```

- Denken (Database Server)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 3e:d3:85:3d:90:c6
```

- Eisen (Load Balancer)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether c2:1d:aa:75:bc:26
```

- Frieren, Flamme, Fern (Laravel Worker)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether c2:1d:aa:75:bc:26

auto eth0
iface eth0 inet dhcp
hwaddress ether a6:e7:3a:6e:16:f2

auto eth0
iface eth0 inet dhcp
hwaddress ether 4a:c6:c1:7e:da:b0
```

- Lawine, Linie, Lugner (PHP Worker)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether c6:1a:8d:dc:d3:3b

auto eth0
iface eth0 inet dhcp
hwaddress ether 72:af:96:19:3b:e0

auto eth0
iface eth0 inet dhcp
hwaddress ether 56:cd:99:d5:c8:a5
```

- Revolte, Richter, Sein, Stark (Client)
```
auto eth0
iface eth0 inet dhcp

auto eth0
iface eth0 inet dhcp
hwaddress ether 36:b7:e7:5f:b4:f3 (DIGUNAKAN UNTUK NOMOR 12 UNTUK TESTING)


auto eth0
iface eth0 inet dhcp

auto eth0
iface eth0 inet dhcp
```

## Sebelum Memulai

- DHCP Relay
```
apt-get update
apt-get install isc-dhcp-relay -y
```

- DHCP Server
```
apt-get update
apt-get install isc-dhcp-server
```

- DNS Server
```
apt-get update
apt-get install bind9 -y
```

- Database Server
```
apt-get update
apt-get install mariadb-server -y
service mysql start
```

- Load Balancer
```
apt-get update
apt-get install nginx -y
apt-get install apache2-utils -y
apt install htop -y
```

- Worker PHP
```
apt-get update
apt-get install nginx -y
apt-get install unzip
apt install nginx php php-fpm -y
```

- Worker Laravel
```
apt-get update
apt-get install mariadb-client -y
# Test connection from worker to database
# mariadb --host=192.216.2.2 --port=3306 --user=kelompoke20 --password=passworde20 dbkelompoke20
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y
```

- Client
```
apt-get update
apt-get install lynx -y
```

## Soal 1
Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

### Script

```
echo '
zone "riegel.canyon.e20.com" {
	type master;
	file "/etc/bind/jarkom/riegel.canyon.e20.com";
};

zone "granz.channel.e20.com" {
	type master;
	file "/etc/bind/jarkom/granz.channel.e20.com";
}; ' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.e20.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.e20.com

echo '
$TTL    604800
@       IN      SOA     riegel.canyon.e20.com. root.riegel.canyon.e20.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@	IN	NS		riegel.canyon.e20.com.
@	IN	A		192.216.4.1     	; IP Fern
@	IN	AAAA		::1
' > /etc/bind/jarkom/riegel.canyon.e20.com

echo '
$TTL    604800
@       IN      SOA     granz.channel.e20.com. root.granz.channel.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@	IN	NS		granz.channel.e20.com.
@	IN	A		192.216.3.1     	; IP Lugner
@	IN	AAAA		::1
' > /etc/bind/jarkom/granz.channel.e20.com

service bind9 restart
```

### Bukti
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/fac226bf-9070-48d4-86a3-b4f21bf76064)

## Soal 2 & 3
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)

### Script
Dijalankan di DHCP Server
```
echo '
INTERFACESv4="eth0"
INTERFACESv6="eth0"
' > /etc/default/isc-dhcp-server
echo '
subnet 192.216.1.0 netmask 255.255.255.0 {
}

subnet 192.216.2.0 netmask 255.255.255.0 {
    option routers 192.216.2.1;
    option broadcast-address 192.216.2.255;
    option domain-name-servers 192.216.1.3;
}

subnet 192.216.3.0 netmask 255.255.255.0 {
    range 192.216.3.16 192.216.3.32;
    range 192.216.3.64 192.216.3.80;
    option routers 192.216.3.10;
    option broadcast-address 192.216.3.255;
    option domain-name-servers 192.216.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.216.4.0 netmask 255.255.255.0 {
    range 192.216.4.12 192.216.4.20;
    range 192.216.4.160 192.216.4.168;
    option routers 192.216.4.10;
    option broadcast-address 192.216.4.255;
    option domain-name-servers 192.216.1.3;
    default-lease-time 720;
    max-lease-time 5760;
} 

host Lugner {
    hardware ethernet 56:cd:99:d5:c8:a5;
    fixed-address 192.216.3.1;
}

host Linie {
    hardware ethernet 72:af:96:19:3b:e0;
    fixed-address 192.216.3.2;
}

host Lawine {
    hardware ethernet c6:1a:8d:dc:d3:3b;
    fixed-address 192.216.3.3;
}

host Fern {
    hardware ethernet 4a:c6:c1:7e:da:b0;
    fixed-address 192.216.4.1;
}

host Flamme {
    hardware ethernet a6:e7:3a:6e:16:f2;
    fixed-address 192.216.4.2;
}

host Frieren {
    hardware ethernet c2:1d:aa:75:bc:26;
    fixed-address 192.216.4.3;
} 

host Denken {
    hardware ethernet 3e:d3:85:3d:90:c6;
    fixed-address 192.216.2.2;
}

host Eisen {
    hardware ethernet c2:1d:aa:75:bc:26;
    fixed-address 192.216.2.3;
} ' > /etc/dhcp/dhcpd.conf
```

## Soal 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

### Script
Sudah menambahkan config pada DHCP Relay diatas
```
subnet 192.216.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.216.3.255;
    option domain-name-servers 192.216.1.3;
    ...
}

subnet 192.216.4.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.216.4.255;
    option domain-name-servers 192.216.1.3;
    ...
} 
```
Dan menjalankan script tersebut di DHCP Relay
```
echo '
SERVERS="192.216.1.2"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=' > /etc/default/isc-dhcp-relay
echo '
net.ipv4.ip_forward=1 ' > /etc/sysctl.conf
service isc-dhcp-relay restart
```

## Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a48c7d79-8bae-40ea-9f16-6a26984e24f8)

## Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
untuk melakukan itu sudah dijalankan di DHCP Server di nomor sebelumnya. Confignya sebagai berikut :

```
subnet 192.216.3.0 netmask 255.255.255.0 {
    ...
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.216.4.0 netmask 255.255.255.0 {
    ...
    default-lease-time 720;
    max-lease-time 5760;
} 
```

## Soal 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3

### Script
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
wget -O '/var/www/html/granz.channel.e20.com.zip' 'https://drive.usercontent.google.com/download?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1'
wait
unzip /var/www/html/granz.channel.e20.com.zip -d /var/www/html/
cp /var/www/html/modul-3/index.php /var/www/html
cp /var/www/html/modul-3/info.php /var/www/html
cp -r /var/www/html/modul-3/css /var/www/html
cp -r /var/www/html/modul-3/js /var/www/html

echo '
server {

 	listen 80;

 	root /var/www/html;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 	location ~ /\.ht {
 			deny all;
 	}
 } ' > /etc/nginx/sites-enabled/default

service nginx start
service php7.2-fpm start
```

### Bukti
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/db2b3181-d450-460d-bfc7-4d65a6ef7ade)

## Soal 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

### Script
Lakukan setup pada DNS Server untuk mengarah ke Load Balancer
```
echo '
$TTL    604800
@       IN      SOA     riegel.canyon.e20.com. root.riegel.canyon.e20.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@	IN	NS		riegel.canyon.e20.com.
@	IN	A		192.216.2.3     	; IP Eisen
@	IN	AAAA		::1
' > /etc/bind/jarkom/riegel.canyon.e20.com

echo '
$TTL    604800
@       IN      SOA     granz.channel.e20.com. root.granz.channel.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@	IN	NS		granz.channel.e20.com.
@	IN	A		192.216.2.3     	; IP Eisen
@	IN	AAAA		::1
' > /etc/bind/jarkom/granz.channel.e20.com
service bind9 restart
```

Kemudian beralih ke Load Balancer untuk melakukan setup
```
echo '
upstream worker  {
	server 192.216.3.1 weight=4; #IP Lugner
	server 192.216.3.2 weight=2; #IP Linie
	server 192.216.3.3 weight=1; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```
Jalankan command di client
```
ab -n 1000 -c 100 -g out.data http://192.216.2.3/
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a854dce0-8b79-47f3-b5cb-6828b3dd5056)

## Soal 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer.

### Script
Ubah konfigurasi pada Load Balancer sesuai algoritma dengan menjalankan script berikut :
- Round Robin
```
echo '
upstream worker  {
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```
- Least Connection
```
echo '
upstream worker  {
	least_conn;
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```
- IP Hash
```
echo '
upstream worker  {
	ip_hash;
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```
- Generic Hash
```
echo '
upstream worker  {
	hash $request_uri consistent;
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```

Kemudian jalankan command pada client
```
ab -n 200 -c 10 -g out.data http://192.216.2.3/
```

### Hasil
- Round Robin
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/5976ba99-6ef0-4a73-8884-8061cc033057)
```Request per second 376.86```
- Least Connection
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/75321fa3-f88d-4aad-aed3-eb3d990a739f)
```Request per second 412.12```
- IP Hash
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/536c2ceb-bedd-41eb-9e34-e0fb9aa8525e)
```Request per second 411.21```
- Generic Hash
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/b73d4c33-6606-4ed8-8d2c-ba26421c2f65)
```Request per second 427.40```
- Grafik
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/b27d6212-e1fc-4776-bb57-229a35a47e7b)

## Soal 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)

### Script
Sebelum melakukannya, pastikan algoritma pada load balancer menggunakan algoritma round robin. Kemudian untuk mengurangi jumlah worker lakukan command pada ```/etc/nginx/sites-enabled/default``` dibagian IP worker satu per satu
Kemudian jalankan command ```ab -n 100 -c 10 -g out.data http://192.216.2.3/``` pada client

### Hasil
- 3 Worker
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/126b3cb7-a7fa-4531-86cb-b69f3da4d04d)
Request persecond 377.37
- 2 Worker
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/0612f579-f39e-4435-9816-a674e438d43a)
Request persecond 402.72
- 1 Worker
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/dafcf9fb-6336-479f-93ac-b41ef03078bc)
Request persecond 412.47
- Grafik
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/9c07e566-4415-4d42-92bc-674f17435f18)

## Soal 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

### Script
Setup beberapa konfigurasi sebagai berikut :
```
mkdir /etc/nginx/rahasisakita
htpasswd -c /etc/nginx/rahasisakita/.htpasswd netics
```
Setelah itu akan diminta untuk input password, lalu ketikan password ajke20. Kemudian jalankan script lagi
```
echo '
upstream worker  {
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	auth_basic "Restricted Area";
	auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
	}

	location ~ /\.ht {
            deny all;
        }
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/8a001507-0645-4378-89c2-b7fccfdc73ea)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/c86f9951-58c4-4d5f-9570-7e2498e4084f)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/4bc3ee0d-8314-487e-81f8-3926e4eafafd)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a3703909-7a61-4de8-8f6b-dcd0f5ba9cac)

## Soal 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id

### Script
Setup pada load balancer dengan script
```
echo '
upstream worker  {
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	auth_basic "Restricted Area";
	auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
	}

location ~ /its {
    proxy_pass https://www.its.ac.id;
    allow all;
}

location ~ /\.ht {
            deny all;
        }
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/b750c017-359c-4c93-943c-e6ac7b54bef5)

## Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168

### Script
Setup pada load balancer dengan script
```
echo '
upstream worker  {
	server 192.216.3.1; #IP Lugner
	server 192.216.3.2; #IP Linie
	server 192.216.3.3; #IP Lawine
}

server {
	listen 80;
	server_name granz.channel.e20.com;

 	location / {
	proxy_pass http://worker;
	auth_basic "Restricted Area";
	auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
  allow 192.216.3.69;
  allow 192.216.3.70;
  allow 192.216.4.167;
  allow 192.216.4.168;
  deny all;
	}

location ~ /its {
    proxy_pass https://www.its.ac.id;
    allow all;
}

location ~ /\.ht {
    deny all;
}
} ' > /etc/nginx/sites-enabled/default

service nginx restart
```

### Hasil
Untuk test harus melakukan fix ip pada salah satu client sebagai perbandingan dengan client yang IP nya tidak diberikan izin. Saya melakukan fix address pada Richter dengan menggunakan IP 69. Untuk melakukannya diperlukan config ulang di DHCP Server. Dengan hasil sebagai berikut :
- Richter
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/102a92e7-d8e7-4d59-82a7-8bf71b09ad26)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/c967ffec-c2d1-4a20-ae56-8260a5832935)

- Revolte

![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/481a70e4-9c00-49db-b7b2-baccf4261a6d)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/ea41b4c8-22c2-4a22-835d-9ccf05ee9f65)

## Soal 13
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern

### Script
Setup Database server Denken terlebih dahulu
```
echo '
[client-server]

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf
```

Lalu ubah ```[bind-address]``` pada file /etc/mysql/mariadb.conf.d/50-server.cnf menjadi ```0.0.0.0``` dimana menerima semua IP. Lalu ```service mysql restart``` untuk restart mysql

Setelah itu jalankan command pada terminal sebagai berikut :
```
mysql
CREATE USER 'kelompoke20'@'%' IDENTIFIED BY 'passworde20';
CREATE USER 'kelompoke20'@'localhost' IDENTIFIED BY 'passworde20';
CREATE DATABASE dbkelompoke20;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoke20'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoke20'@'localhost';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a39a2532-34d4-4a28-96fe-c8dcbccc9078)

Pada Laravel worker jalankan command ```mariadb --host=192.216.2.2 --port=3306 --user=kelompoke20 --password``` dengan password ```passworde20```. Kemudian jalankan ```mysql``` dan ketik ```SHOW DATABASES;``` maka akan muncul database yang sudah terhubung dengan Database Server
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/32431f69-bd3c-4c2d-af57-e8d434a2197e)

## Soal 14
Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

### Script
Install composer dengan script
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt update && apt upgrade
apt-get install wget -y
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
```
Kemudian install git dan clone dari github yang sudah diberikan
```
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
```

Lakukan konfigurasi pada file env pada laravel sebagai berikut
```
cp /var/www/laravel-praktikum-jarkom/.env.example /var/www/laravel-praktikum-jarkom/.env
echo '
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.216.2.2
DB_PORT=3306
DB_DATABASE=dbkelompoke20
DB_USERNAME=kelompoke20
DB_PASSWORD=passworde20

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env

cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```

Setelah berhasil melakukan konfigurasi maka selanjutnya setup nginx pada semua worker laravel dengan port sebagai berikut :
```
192.216.4.1:8001; # Fern 
192.216.4.2:8002; # Flamme
192.216.4.3:8003; # Frieren
```
konfigurasi nginx
```
echo 'server {
    listen {PORT};

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' > /etc/nginx/sites-enabled/laravel-worker
service nginx start
```
```{PORT}``` diganti sesuai dengan laravel workernya

### Hasil
Jalankan command ```lynx localhost:{PORT}```
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/dcea25e4-7b7e-49be-901f-7a9825e57f19)

## Soal 15, 16, dan 17
Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register (15)
POST /auth/login (16)
GET /me (17)

### Script
- No 15
Disini kami akan menggunakan worker laravel Fern yang nantinya akan menjadi worker yang akan ditesting oleh client ```Revolte```. Sebelum dilakukan testing, kami menggunakan bantuan file ```.json``` yang akan digunakan sebagai body yang akan dikirim pada endpoint ```/api/auth/register``` nantinya sebagai berikut :
```
echo '
{
  "username": "kelompoke20",
  "password": "passworde20"
}' > register.json
```
Lalu lakukan benchmark dengan menjalankan command sebagai berikut
```ab -n 100 -c 10 -p register.json -T application/json http://192.216.4.1:8001/api/auth/register```
Untuk mendapat response lakukan command ```curl -X POST -H "Content-Type: application/json" -d @register.json http://192.216.4.1:8001/api/auth/register -o register_response.txt```

### Hasil 15
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/7199eeef-0b86-461e-8d3b-94ca89998173)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/c6ffbb16-1653-4c85-85e6-1e66750d945c)

- No 16
Disini kami akan menggunakan worker laravel Fern yang nantinya akan menjadi worker yang akan ditesting oleh client ```Revolte```. Sebelum dilakukan testing, kami menggunakan bantuan file ```.json``` yang akan digunakan sebagai body yang akan dikirim pada endpoint ```/api/auth/login``` nantinya sebagai berikut :
```
echo '
{
  "username": "kelompoke20",
  "password": "passworde20"
}' > register.json
```
Lalu lakukan benchmark dengan menjalankan command sebagai berikut
```ab -n 100 -c 10 -p register.json -T application/json http://192.216.4.1:8001/api/auth/login```
Untuk mendapat response lakukan command ```curl -X POST -H "Content-Type: application/json" -d @register.json http://192.216.4.1:8001/api/auth/login -o register_response.txt```

### Hasil 16
Hasil Apache Benchmark
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a2cec87c-32b4-4253-b642-22d58cafaab7)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/9570f8ea-dfaf-426c-bf63-cbe5c43dd981)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/c11d4552-f665-405a-a984-86ad7fc3aad6)

- No 17
Disini kami akan menggunakan worker laravel Fern yang nantinya akan menjadi worker yang akan ditesting oleh client ```Revolte```. Sebelum dilakukan testing. Ada beberapa konfigurasi yang harus disiapkan sebagai berikut :
Dapatkan token terlebih dahulu dari /api/me
```
curl -X POST -H "Content-Type: application/json" -d @login.json http://192.216.4.1:8001/api/auth/login > login_output.txt
```
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/32c8eff0-4f0e-4110-8ab2-8a43919c9bcb)
Lalu jalankan perintah berikut
```token=$(cat login_output.txt | jq -r '.token')```
Setelah itu lakukan Apache Benchmark
```ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.216.4.1:8001/api/me```
Untuk mendapatkan response, lakukan curl seperti berikut
```curl -X GET -H "Authorization: Bearer $token" http://192.216.4.1:8001/api/me > get_response.txt```
### Hasil 17
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/056c13f6-d945-47aa-bfe5-277a24842c47)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/02a0eb0d-ce9b-49b9-8f31-9d7c010fbac6)

## Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern

### Script
Lakukan setup nginx pada load balancer
```sh
echo 'upstream worker {
    server 192.216.4.1:8001;
    server 192.216.4.2:8002;
    server 192.216.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.e20.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-enabled/laravel-worker

service nginx restart
```

### Hasil
Lakukan Apache Benchmark
```ab -n 100 -c 10 -p login.json -T application/json http://riegel.canyon.e20.com/api/auth/login```
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/5f9985e8-7b53-48c8-a4d1-4afd6e2b10b6)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/c2fefda2-a507-46ed-b5e0-7e9f088c41e2)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/d476e36c-5745-44d5-ab4e-31cc7b459667)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/19fc7bc5-37c4-45f4-acb9-a1e71d7ef708)

## Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)

### Script 1
```sh
# Setup Awal
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

### Script 2
```sh
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

### Script 3
```sh
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/a9a02d62-648d-419e-af49-eb6eb86e138f)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/b5f8d4e1-e723-4b87-9af4-fe12021eeb16)
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/af30825d-a92a-4c53-b21f-9f997bfb2381)

## Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second

### Script
```sh
echo 'upstream backend {
    least_conn;
    server 192.216.4.1:8001; # IP Fern
    server 192.216.4.2:8002; # IP Flamme
    server 192.216.4.3:8003; # IP Frieren
}

server {
    listen 80;
    server_name riegel.canyon.e20.com;

    location / {
	proxy_pass http://backend;
    }
    error_log /var/log/nginx/lb_laravel_error.log;
    access_log /var/log/nginx/lb_laravel_access.log;
}
' > /etc/nginx/sites-available/default

ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
service nginx restart
```

### Hasil
![image](https://github.com/gustino7/Jarkom-Modul-3-E20-2023/assets/93267604/bbdfb10b-35bc-4bde-a412-9db30786d391)
