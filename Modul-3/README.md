# Laporan Resmi Modul 3 

## Soal 1

## Soal 2

## Soal 3

## Soal 4

## Soal 5

## Soal 6

## Soal 7

**User authentication untuk Anri**

- Pada UML Mojokerto (proxy server) install squid dan apache2-utils
  ```
  apt-get install squid
  apt-get install apache2-utils
  ```
- Setup username dan password\
  User: userta_e06\
  Password: inipassw0rdta_e06
  ```
  htpasswd -c /etc/squid/passwd userta_e06
  ```
  <img src="Img/soal7.PNG" width="600px">
  
- Ubah konfigurasi squid
  ```
  nano /etc/squid/squid.conf
  ```
  ```
  http_port 8080
  visible_hostname mojokerto

  auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
  auth_param basic children 5
  auth_param basic realm Proxy
  auth_param basic credentialsttl 2 hours
  auth_param basic casesensitive on
  acl USERS proxy_auth REQUIRED
  http_access allow USERS
  ```
  
## Soal 8 dan 9

**Filtering jadwal pengaksesan**

- Mendefinisikan jadwal yang tidak restricted. Dalam soal 8 dan 9, yang tidak restricted adalah:
  + Selasa 13:00 - 18:00, 21:00 - 23:59
  + Rabu 00:00 - 09:00, 13:00 - 18:00, 21:00 - 23:59
  + Kamis 00:00 - 09:00, 21:00 - 23:59
  + Jumat 00:00 - 09:00
  
  <img src="Img/soal89.PNG" width="600px">

- Menambah konfigurasi squid
  ```
  nano /etc/squid/squid.conf
  ```
  ```
  include /etc/squid/acl.conf
  http_access allow work 
  http_access allow bimbingan1
  http_access allow bimbingan2
  http_access deny all
  ```
  
## Soal 10

**Redirect ke monta.if.its.ac.id setiap mengakses google.com**

- Mendefinisikan google.com sebagai website restricted
  ```
  nano /etc/squid/restrict-sites.acl
  ```
  ```
  google.com
  ```

- Menambah konfigurasi squid
  ```
  acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
  deny_info http://monta.if.its.ac.id BLACKLISTS
  http_access deny BLACKLISTS
  ```

## Soal 11

**Mengganti error page default squid**

- Mendownload page ke directory error template dan extract isinya
  ```
  cd /usr/share/squid/errors
  wget 10.151.36.202/error403.tar.gz
  tar -xvf error403.tar.gz
  ```
- Meng-copy isi /usr/share/squid/errors/template ke /usr/share/squid/errors/error403
  ```
  cp template/* error403
  ```
- Mengganti /usr/share/squid/errors/error403/ERR_ACCESS_DENIED dengan /usr/share/squid/errors/error403/error403.html
  ```
  mv error403/error403.html error403/ERR_ACCESS_DENIED
  ```
- Memindahkan /usr/share/squid/errors/error403/error.jpg ke /usr/share/squid/icons
  ```
  mv error403/error.jpg ../../icons
  ```
- Mengubah directory background image di stylesheet error403/ERR_ACCESS_DENIED
  ```
  body {
    background: url('/squid-internal-static/icons/error.jpg');
  }
  ```
- Menambah konfigurasi squid
  ```
  error_directory /usr/share/squid/errors/error403/
  icon_directory /usr/share/squid/icons/
  ```
  
  <img src="Img/soal11_show.PNG" width="600px">
  
  **Catatan**: Dari inspect page, halaman yang ditampilkan sudah benar, namun image tidak dapat ditampilkan.

## Soal 12
