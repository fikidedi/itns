# IT Networking Support

Berikut ini adalah step-step konfigurasi server dengan menggunakan debian linux

## 1. Installasi dan Partisi
- 1 buah partisi untuk /
- 1 buah partisi untuk swap (4GB aja)
- 3 buah partisi 5GB nanti dibuat tipe RAID 5
- mount point buat sebagai /files (buat secara manual)

## 2. Konfigurasi IP
eth0 = 172.70.55.3/29
artinya
ip addressnya 172.70.55.3
netmask 255.255.255.248

## 3. Cara Buat User Linux
> Masuk ke user root dulu

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
