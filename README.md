# Lapres_Modul3_A2


    Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
    (1) Client di subnet 2 mendapatkan peminjaman alamat IP dengan range 192.168.0.2 s.d. 192.168.0.10 dan 192.168.0.20 s.d. 192.168.0.30 dengan netmask 255.255.255.0.
    (2) Client di subnet 3 mendapatkan peminjaman alamat IP dengan range dari 192.168.1.2 s.d. 192.168.1.99 dengan netmask 255.255.255.0.
    (3) Client di subnet 2 mendapatkan peminjaman alamat IP selama 10 menit dan client di subnet 3 mendapatkan peminjaman alamat IP selama 5 menit.
    (4) Masing-masing client harus mendapatkan konfigurasi DNS / nameserver yang terdiri atas IP ARTICUNO, 202.46.129.2, dan 10.151.36.7 secara otomatis.
    (5) Client PSYDUCK selalu mendapatkan IP 192.168.1.28, TANPA menggunakan konfigurasi IP Statis.

Prof. Oak tidak ingin jaringan lokalnya terhubung ke Internet secara langsung. Ia ingin jaringan lokalnya melalui sebuah medium yang dapat menyaring dan membatasi sebelum terhubung ke Internet, sehingga ia meminta anda membuatkan Proxy untuk kota Pallet. Pertama, Prof. Oak ingin orang yang bisa mengakses internet melalui proxy hanyalah penduduk kota Pallet dan trainer yang telah mendaftar di gym.

(6) Prof. Oak meminta anda membuatkan user otentikasi dengan format:

● User : yy_moltres_user ● Password : yy_password

Note: yy adalah nama kelompok masing-masing. Contoh : a1_moltres_user Beberapa bulan setelah mendapatkan akses internet, para trainer (subnet 2) menjadi malas melatih pokemonnya karena terlalu asyik bermain Pokemon Go sehingga para pokemon menjadi lemah. Selain itu, para penduduk (subnet 3) menjadi malas untuk membuka toko mereka karena terlalu asyik menonton YouTube Atta Halilintar. Karena permasalahan tersebut, anda diminta membatasi akses internet untuk penduduk kota Pallet dengan pembatasan akses proxy sebagai berikut:

● (7) Untuk Client pada subnet 2 HANYA BISA mengakses internet pada hari Senin s.d. Jumat pukul 11.00 s.d. 13.00 WIB ● (8) Untuk Client pada subnet 3 HANYA BISA mengakses internet pada hari Senin s.d. Jumat pukul 20.00 s.d. 07.00 WIB pada keesokan harinya.

Prof. Oak mendapat informasi dari salah satu trainer bahwa google.com merupakan search engine buatan tim Rocket. Oleh karena itu, Prof. Oak memerintahkan anda untuk membuat aturan (9) setiap ada koneksi dari subnet AJK (10.151.36.0/24) yang mengakses google.com akan langsung dialihkan menuju duckduckgo.com. (10) Selain itu, demi menjaga keamanan kota Pallet, anda diminta untuk mem-block setiap koneksi yang berasal dari subnet Informatics_wifi (10.151.252.0/22) menuju http://10.151.36.5:5000, sebab subnet Informatics_wifi adalah milik tim Rocket. (11) Karena menurut Prof. Oak menghafalkan IP Proxy kota Pallet cukup merepotkan, kalian diminta untuk mempermudah trainer dan penduduk dalam menggunakan Proxy yaitu cukup dengan mengetikkan proxy.yy.com dan memasukkan port 8888.

Langkah-Langkah

topologi.sh
![topologi](https://user-images.githubusercontent.com/47860298/67634968-66af8b80-f8f4-11e9-953d-8c008eff1d35.png

Install isc-dhcp-server di MEWTWO 
`apt-get install isc-dhcp-server`

Install isc-dhcp-relay di PIKACHU
`apt-get install isc-dhcp-relay`

-Setelah berhasil, masukkan IP MEWTWO (10.151.73.67) sesuai yang diminta DHCP relay.

-Masukkan interface yang diminta dengan `eth1 eth2 eth3`

1. Client di subnet 2 mendapatkan peminjaman alamat IP dengan range 192.168.0.2 s.d. 192.168.0.10 dan 192.168.0.20 s.d. 192.168.0.30 dengan netmask 255.255.255.0.

2. Client di subnet 3 mendapatkan peminjaman alamat IP dengan range dari 192.168.1.2 s.d.
192.168.1.99 dengan netmask 255.255.255.0.

3. Client di subnet 2 mendapatkan peminjaman alamat IP selama 10 menit dan client di
subnet 3 mendapatkan peminjaman alamat IP selama 5 menit.

4. Masing-masing client harus mendapatkan konfigurasi DNS / nameserver yang terdiri atas
IP ARTICUNO , 202.46.129.2 , dan 10.151.36.7 secara otomatis.

5. Client PSYDUCK selalu mendapatkan IP 192.168.1.28, TANPA menggunakan
konfigurasi IP Statis.

Jawab: Pada MEWTWO

-Buka `nano /etc/default/isc-dhcp-server` dan sesuaikan dengan gambar dibawah.
![image](https://user-images.githubusercontent.com/37492916/67634136-aec9b080-f8ea-11e9-885f-1c0cdb4be1e6.png)

-Buka `nano /etc/dhcp/dhcpd.conf` lalu ketikkan config seperti dibawah.

```
subnet 10.151.73.64 netmask 255.255.255.248 {
} 

//NOMOR 1
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.73.26, 202.46.129.2, 10.151.36.7; // NOMOR4
    default-lease-time 600; //NOMOR 3
    max-lease-time 7200;
} 

//NOMOR 2
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.2 192.168.0.10;
    range 192.168.0.20 192.168.0.30;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.73.26, 202.46.129.2, 10.151.36.7;
    default-lease-time 600;
    max-lease-time 7200;
}


//NOMOR 5
host psyduck {
    hardware ethernet ee:44:8f:8a:65;9c;
    fixed-address 192.168.1.28;
} 
```

-Setting semua IP Client di `nano /etc/network/interface` seperti berikut

```
auto eth0
iface eth0 inet dhcp
```

(khusus Client PSYDUCK tuliskan settingan seperti berikut)

```
auto eth0
iface eth0 inet dhcp
hwaddress ether ee:44:8f:8a:65:9c
```

Restart service isc-dhcp-server dengan perintah
`service isc-dhcp-server restart`

Jika terjadi failed!, maka stop dulu, kemudian start kembali

```
service isc-dhcp-server stop
service isc-dhcp-server start
```


//Nomor 6 

● User : yy_moltres_user

● Password : yy_password

Note: yy adalah nama kelompok masing-masing. Contoh : a1_moltres_user


Jawab:

- Install squid3 pada UML MOLTRES `apt-get install squid3`

- Install apache2-utils pada UML MOLTRES `apt-get install apache2-utils`

- Backup terlebih dahulu file konfigurasi default yang disediakan squid. Ketikkan perintah berikut untuk melakukan backup:

`mv /etc/squid3/squid.conf /etc/squid3/squid.conf.bak`

- Buat user dan password baru. Ketikkan:

`htpasswd -c /etc/squid3/passwd a2_moltres_user` dan `password: a2_password`

-Buat konfigurasi baru dengan mengetikkan:

`nano /etc/squid3/squid.conf`

Tambahkan konfigurasi seperti di bawah ini

```
http_port 7777
visible_hostname mewtwo

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

- Restart squid dengan cara mengetikkan perintah:

`service squid3 restart`



//Nomor7
![jarkom6](https://user-images.githubusercontent.com/47860298/67634432-71672200-f8ee-11e9-805d-f649ec59a277.png)

untuk mencobanya silahkan menginstall elinks dan config-kan elinks pada client.

![elinkscubone](https://user-images.githubusercontent.com/47860298/67634598-bc823480-f8f0-11e9-907b-aaf6cacbe019.png)

silahkan ganti date pada server sesuai ketentuan soal

![gantiwaktu](https://user-images.githubusercontent.com/47860298/67634654-5d70ef80-f8f1-11e9-9e29-faad893957a3.png)

jika benar maka elinks bisa mengakses web yang dituju

![hasilcubone](https://user-images.githubusercontent.com/47860298/67634767-9cec0b80-f8f2-11e9-87c5-1a33d87ee3d1.png)

//Nomor 8

Jawab:

-Buka `nano /etc/squid3/squid.conf` lalu ubah seperti berikut:

![jarkom6](https://user-images.githubusercontent.com/47860298/67634432-71672200-f8ee-11e9-805d-f649ec59a277.png)

-untuk mencobanya silahkan menginstall elinks dan config-kan elinks pada client.

![elinkspsyduck](https://user-images.githubusercontent.com/47860298/67635139-ebe77000-f8f5-11e9-9101-4c4e77e1ce2a.png)

-silahkan ganti date pada server sesuai ketentuan soal

-jika benar maka elinks bisa mengakses web yang dituju


//Nomor 11

Jawab:

-Install aplikasi bind9 pada ARTICUNO dengan perintah: `apt-get install bind9 -y`

-Lakukan perintah pada ARTICUNO. Isikan seperti berikut: `nano /etc/bind/named.conf.local`

-Isikan konfigurasi domain proxy.a2.com sesuai dengan syntax berikut:

```
zone "proxy.a2.com"{
	type master;
	file "/etc/bind/jarkom/proxy.a2.com";
	};
 ```
 
-Buat folder jarkom di dalam /etc/bind `mkdir /etc/bind/jarkom`
 
-Copykan file db.local pada path /etc/bind ke dalam folder proxy.a2.com yang baru saja dibuat dan diubah namanya menjadi jarkom `cp /etc/bind/db.local /etc/bind/jarkom/proxy.a2.com`

-Buka `nano /etc/bind/jarkom/proxy.a2.com` dan ubah seperti gambar di bawah ini:

![proxya2](https://user-images.githubusercontent.com/47860298/67635069-34eaf480-f8f5-11e9-9d3e-b2331eeef605.png)
 
-Restart bind9 dengan perintah `service bind9 restart`






