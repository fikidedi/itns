# IT Networking Support

STEP-STEP KONFIGURASI CORESRV

Daftar Isi
[Konfigurasi DNS Server (BIND](#bind "Baca Konfigurasi DNS Server")



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

**root@coresrv:~#**nano /etc/network/interfaces

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

## 4. [Konfigurasi DNS Server (BIND)](#bind)

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
Copy file yang dibutuhkan 

**root@coresrv:/etc/bind#**cp db.local db.domain1

**root@coresrv:/etc/bind#**cp db.local db.domain2

**root@coresrv:/etc/bind#**cp db.127 db.ip

Kemudian edit file domain1

**root@coresrv:/etc/bind#**nano db.domain1
edit dan tambahkan script berikut

gunakan ctrl+w -> ctrl+r untuk mereplace kata localhost ganti menjadi lkskabupatentubaba.net

```
@	IN	NS 	lkskabupatentubaba.net.
@	IN	A	172.70.55.3
files	IN	A	172.70.55.3
internal	IN	A	172.70.55.3
www	IN	A	172.70.55.1
```
**root@coresrv:/etc/bind#**nano db.domain2

edit dan tambahkan script berikut

gunakan ctrl+w -> ctrl+r untuk mereplace kata localhost ganti menjadi lkskabupaten2019.id

```
@	IN	NS 	lkskabupaten2019.id.
@	IN	A	172.70.55.4
monitor	IN	A	172.70.55.4
vpn	IN	A	172.70.55.4
```

**root@coresrv:/etc/bind#**nano db.ip
edit dan tambahkan script berikut

gunakan ctrl+w -> ctrl+r untuk mereplace kata localhost ganti menjadi lkskabupatentubaba.net

```
@	IN	NS 	lkskabupatentubaba.net.
3	IN	PTR	lkskabupatentubaba.net.
4	IN	PTR	lkskabupaten2019.id.
1	IN	PTR	www.lkskabupatentubaba.net.
```

restart bind9 dengan perintah berikut, pilih salah satu

**root@coresrv:/etc/bind#**service bind9 restart

**root@coresrv:/etc/bind#**/etc/init.d/bind9 restart

ubah file resolv.conf

**root@coresrv:/etc/bind#**nano /etc/resolv.conf

edit dan ganti dengan
```
nameserver 127.0.0.1
```
untuk ceking bisa gunakan perintah nslookup
