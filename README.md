# IT Networking Support

STEP-STEP KONFIGURASI CORESRV

## 1. Installasi dan Partisi
- 1 buah partisi untuk /
- 1 buah partisi untuk swap (4GB aja)
- 3 buah partisi 5GB nanti dibuat tipe RAID 5
- mount point buat sebagai /files (buat secara manual)

> Pastikan masuk ke user root sebelum melakukan konfigurasi

## 2. Konfigurasi IP
eth0 = 172.70.55.3/29 artinya
ip addressnya 172.70.55.3
netmask 255.255.255.248

**root@coresrv:~#**nano /etc/networking/interface

edit dan tambahkan script berikut
```
auto eth0
iface eth0 inet static
	address 172.70.55.3
	netmask 255.255.255.248
```
restart dengan perintah (pilih salah satu)
**root@coresrv:~#**/etc/init.d/networking restart
**root@coresrv:~#**service networking restart


## 3. Cara Buat User Linux

**root@debian:~#**nano user.sh

ketik dan simpan script berikut
```
for((id=1; id<=30; id++))
  mkdir -p /home/user$id
  useradd user$id -d /home/user$id -s /bin/bash
  passwd user$id <<< "lks2020"$'\n'"lks2020"
done
```
**root@debian:~#**chmod +x user.sh

**root@debian:~#**bash user.sh

untuk mengecek bisa lewat *nano /etc/password* atau dengan cara login tiap user

## 4. Konfigurasi DNS Server (BIND)

**root@coresrv:~#**apt-get install bind9

**root@coresrv:~#**cd /etc/bind

**root@coresrv:/etc/bind#**nano named.conf.default-zones

tambahkan dan simpan script berikut dibagian bawah
```
zone "lkskabupatentubaba.net"{
	type master;
	file "/etc/bind/db.domain1";
};
zone "lkskabupaten2019.id"{
	type master;
	file "/etc/bind/db.domain2";
};
zone "55.70.172.in-addr.arpa"{
	type master;
	file "/etc/bind/db.ip";
};
```
kopi file yang dibutuhkan 

**root@coresrv:/etc/bind#**cp db.local db.domain1

**root@coresrv:/etc/bind#**cp db.local db.domain2

**root@coresrv:/etc/bind#**cp db.127 db.ip

edit file domain1

**root@coresrv:/etc/bind#**nano db.domain1
