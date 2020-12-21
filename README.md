# Jarkom_Modul5_Lapres_T06
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
![CIDR Topologi](https://user-images.githubusercontent.com/61286109/102782210-8c86d200-43cb-11eb-8316-12f8969fb196.png) <br>
Hasil yang didapat adalah <b>Netmask /22</b> untuk subnet besar topologi diatas.
- Pembagian IP dengan pohon berdasarkan penggabungan subnet yang telah dilakukan.
![Tree CIDR Modul5](https://user-images.githubusercontent.com/61286109/102798176-4e95a800-43e3-11eb-9ed1-dfc4c61958e1.png) <br>
- Dari pohon tersebut akan mendapat pembagian IP sebagai berikut. <br>
![Pembagian IP](https://user-images.githubusercontent.com/61286109/102786622-657fce80-43d2-11eb-9fcc-d6e9866b04ca.PNG) <br>

#### Konfigurasi Tiap UML
1. Pertama, membuat file `topologi.sh`.
![NanoTopologi](https://user-images.githubusercontent.com/61286109/102784082-73cbeb80-43ce-11eb-89e5-5bfb656220a9.PNG) <br>
2. Pada semua UML Router, ke file `nano /etc/sysctl.conf` kemudian uncomment `net.ipv4.ip_forward=1` dan aktifkan dengan syntax `sysctl -p`. <br>
3. Lakukan setting interface pada semua UML di `nano /etc/network/interfaces` dan di restart dengan syntax `service networking restart`. <br>
- ROUTER SURABAYA <br>
![iface surabaya](https://user-images.githubusercontent.com/61286109/102826860-1064ac80-4414-11eb-86dc-7c5a0ffeeb4b.PNG) <br>
- ROUTER KEDIRI <br>
![iface kediri](https://user-images.githubusercontent.com/61286109/102826853-0d69bc00-4414-11eb-92cf-ad9fc9490240.PNG) <br>
- ROUTER BATU <br>
![iface batu](https://user-images.githubusercontent.com/61286109/102826849-0b076200-4414-11eb-9c59-e20ba3aa504f.PNG) <br>
- SERVER MADIUN <br>
![iface madiun](https://user-images.githubusercontent.com/61286109/102826854-0e025280-4414-11eb-86aa-4edcd9ba5d04.PNG) <br>
- SERVER PROBOLINGGO <br>
![iface probolinggo](https://user-images.githubusercontent.com/61286109/102826858-0f337f80-4414-11eb-835d-8c48f2292a8f.PNG) <br>
- SERVER MALANG <br>
![iface malang](https://user-images.githubusercontent.com/61286109/102826855-0e025280-4414-11eb-99d3-ecab6d9f06a6.PNG) <br>
- SERVER MOJOKERTO <br>
![iface mojokerto](https://user-images.githubusercontent.com/61286109/102826857-0e9ae900-4414-11eb-8f4e-2465a7cf0df6.PNG) <br>
- KLIEN GRESIK <br>
![iface gresik](https://user-images.githubusercontent.com/61286109/102826852-0cd12580-4414-11eb-8710-1234c41f2c70.PNG) <br>
- KLIEN SIDOARJO <br>
![iface sidoarjo](https://user-images.githubusercontent.com/61286109/102826859-0fcc1600-4414-11eb-9b38-f57858a26aa6.PNG) <br>

