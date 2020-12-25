<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 5 - FIREWALL <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## SOAL
<b>(A)</b> Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini : <br>
![Topologi Modul 5](https://user-images.githubusercontent.com/61286109/102757895-8d583d80-43a4-11eb-971a-50f803ca820a.PNG) <br>
Keterangan : 
- <b>SURABAYA</b> diberikan <b>IP TUNTAP</b>
- <b>MALANG</b> merupakan <b>DNS Server</b> diberikan <b>IP DMZ</b>
- <b>MOJOKERTO</b> merupakan <b>DHCP Server</b> diberikan <b>IP DMZ</b>
- <b>MADIUN</b> dan <b>PROBOLINGGO</b> merupakan <b>WEB Server</b>
- <b>Setiap Server</b> diberikan memory sebesar <b>128M</b>
- <b>Client dan Router</b> diberikan memori sebesar <b>96M</b>
- Jumlah host pada subnet <b>SIDOARJO 200 Host</b>
- Jumlah host pada subnet <b>GRESIK 210 Host</b> 
<br>
<b>(B)</b> karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik <b>CIDR</b> atau <b>VLSM</b>. Setelah melakukan subnetting, <b>(C)</b> kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung. 
<br>
<br>
<b>(D)</b> Tugas berikutnya adalah memberikan ip pada subnet <b>SIDOARJO</b> dan <b>GRESIK</b> secara dinamis menggunakan bantuan DHCP SERVER (Selain subnet tersebut menggunakan ip static). Kemudian kalian mengingat bahwa kalian harus setting DHCP RELAY pada router yang menghubungkannya, seperti yang kalian telah pelajari di masa lalu. 
<br>
<br>
<b>(1)</b> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi <b>SURABAYA</b> menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE. 
<br>
<br>
<b>(2)</b> Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada <b>SURABAYA</b> demi menjaga keamanan. <br> <br>
<b>(3)</b> Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan <b>iptables pada masing masing server</b>, selebihnya akan di DROP. 
<br> <br>
kemudian kalian diminta untuk membatasi akses ke <b>MALANG</b> yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: <br>
<b>(4)</b> Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. <br>
<b>(5)</b> Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. <br>
Selain itu paket akan di REJECT. 
<br> <br>
Karena kita memiliki 2 buah <b>WEB Server</b>, <b>(6)</b> Bibah ingin <b>SURABAYA</b> disetting sehingga setiap request dari client yang mengakses <b>DNS Server</b> akan didistribusikan <b>secara bergantian</b> pada <b>PROBOLINGGO</b> port 80 dan <b>MADIUN</b> port 80. 
<br> <br>
<b>(7)</b> Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop. <br>
Bibah berterima kasih kepada kalian karena telah mau membantunya. Bibah juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

## PENYELESAIAN <br>
### Menggunakan Metode CIDR
#### Subnetting (Pembagian IP) <br>
- Menentukan subnet yang ada dalam topologi dan lakukan labelling netmask terhadap masing-masing subnet kemudian menggabungkan subnet-subnet sampai menjadi subnet besar. <br>
![SubnetTopologi Modul 5](https://user-images.githubusercontent.com/61286109/102895716-c7eed280-4497-11eb-9344-008e4d53366b.png) <br>
Hasil yang didapat adalah <b>Netmask /21</b> untuk subnet besar topologi diatas.
- Pembagian IP dengan pohon berdasarkan penggabungan subnet yang telah dilakukan.
![TreeCIDR Modul5](https://user-images.githubusercontent.com/61286109/102901541-58311580-44a0-11eb-9e8e-62e186f6eda4.png) <br>
- Dari pohon tersebut akan mendapat pembagian IP sebagai berikut. <br>
![Pembagian IP](https://user-images.githubusercontent.com/61286109/102895979-3fbcfd00-4498-11eb-8706-a60ed977048d.PNG) <br>

#### Konfigurasi Tiap UML
1. Pertama, membuat file `topologi.sh`.
![NanoTopologi](https://user-images.githubusercontent.com/61286109/102784082-73cbeb80-43ce-11eb-89e5-5bfb656220a9.PNG) <br>
2. Pada semua UML Router, ke file `nano /etc/sysctl.conf` kemudian uncomment `net.ipv4.ip_forward=1` dan aktifkan dengan syntax `sysctl -p`. <br>
3. Lakukan setting interface pada semua UML di `nano /etc/network/interfaces` dan di restart dengan syntax `service networking restart`. <br>
- ROUTER SURABAYA <br>
![iface surabaya](https://user-images.githubusercontent.com/61286109/102900032-55352580-449e-11eb-8844-8224bb6b9db2.PNG) <br>
- ROUTER KEDIRI <br>
![iface kediri](https://user-images.githubusercontent.com/61286109/102899925-3767c080-449e-11eb-8854-e8714623d1c4.PNG) <br>
- ROUTER BATU <br>
![iface batu](https://user-images.githubusercontent.com/61286109/102899633-d0e2a280-449d-11eb-95ed-e3ea0a41ca32.PNG) <br>
- SERVER MADIUN <br>
![iface madiun](https://user-images.githubusercontent.com/61286109/102902989-4badbc80-44a2-11eb-972d-58845f439e48.PNG) <br>
- SERVER PROBOLINGGO <br>
![iface probolinggo](https://user-images.githubusercontent.com/61286109/102902985-494b6280-44a2-11eb-9da6-de197f677452.PNG) <br>
- SERVER MALANG <br>
![iface malang](https://user-images.githubusercontent.com/61286109/102900739-4ef37900-449f-11eb-9809-13a3a6b25ac0.PNG) <br>
- SERVER MOJOKERTO <br>
![iface mojokerto](https://user-images.githubusercontent.com/61286109/102900727-4bf88880-449f-11eb-8bfb-4095fa168432.PNG) <br>
- KLIEN GRESIK <br>
![iface gresik](https://user-images.githubusercontent.com/61286109/102826852-0cd12580-4414-11eb-8710-1234c41f2c70.PNG) <br>
- KLIEN SIDOARJO <br>
![iface sidoarjo](https://user-images.githubusercontent.com/61286109/102826859-0fcc1600-4414-11eb-9b38-f57858a26aa6.PNG) <br>
4. Lakukan Routing terhadap Router Surabaya dengan membuat file dengan `nano routing.sh` untuk memudahkan kita karena pada saat setiap UML di restart maka routenya ke reset. Untuk menjalankannya ketik syntax `source routing.sh`.
- ROUTING SURABAYA <br>
![routing surabaya](https://user-images.githubusercontent.com/61286109/102912675-47889b80-44b0-11eb-9f89-5b2f4a1d4440.PNG) <br>
- ROUTING KEDIRI <br>
![routing kediri](https://user-images.githubusercontent.com/61286109/102912665-45bed800-44b0-11eb-9509-163626b42c1c.PNG) <br>

5. Install DHCP server, DHCP Relay, DNS server dan Web server
- MALANG (DNS Server) <br>
<img width="364" alt="dns server" src="https://user-images.githubusercontent.com/26424136/102977306-63825080-4535-11eb-9e09-a4fd4849cc57.PNG"> <br>
- MOJOKERTO (DHCP Server) <br>
<img width="369" alt="dhcp server" src="https://user-images.githubusercontent.com/26424136/102977301-61b88d00-4535-11eb-903e-588d5cc6c906.PNG"> <br>
- SURABAYA, BATU dan KEDIRI (DHCP Relay) <br>
BELUM DI SS <br>
- MADIUN dan PROBOLINGGO (Web Server) <br>
BELUM DI SS


#### NOMOR 1
10.151.72.90 merupakan ip eth0 SURABAYA <br>
<img width="417" alt="no1" src="https://user-images.githubusercontent.com/26424136/103143256-078e1680-4745-11eb-8c75-d838785b642c.PNG">

#### NOMOR 2
10.151.73.176 merupakan IP DMZ <br>
<img width="364" alt="no2" src="https://user-images.githubusercontent.com/26424136/103143257-09f07080-4745-11eb-8f3f-b1de2c5578e0.PNG">

#### NOMOR 3
Membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan
- UML Mojokerto <br>
<img width="383" alt="no3" src="https://user-images.githubusercontent.com/26424136/103143258-0bba3400-4745-11eb-8f90-e9c586d754ad.PNG">
- UML Malang <br>
<img width="384" alt="no3MLG" src="https://user-images.githubusercontent.com/26424136/103143259-0e1c8e00-4745-11eb-9f0b-071e289148df.PNG">

#### NOMOR 4
Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat, Selain itu di <b>reject</b>. <br>
<img width="366" alt="no4" src="https://user-images.githubusercontent.com/26424136/103143260-0f4dbb00-4745-11eb-911b-dc2245953f3b.PNG">

#### NOMOR 5
Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya, Selain itu di <b>reject</b>.<br>
<img width="412" alt="no5" src="https://user-images.githubusercontent.com/26424136/103143261-107ee800-4745-11eb-829a-d9d62cacc2fa.PNG">
