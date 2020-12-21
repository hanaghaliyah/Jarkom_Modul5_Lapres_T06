# Jarkom_Modul5_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 5 - FIREWALL <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## TOPOLOGI UML <br>
![Topologi Modul 5](https://user-images.githubusercontent.com/61286109/102757895-8d583d80-43a4-11eb-971a-50f803ca820a.PNG) <br>
## PENYELESAIAN <br>
### Menggunakan Metode CIDR
#### Subnetting (Pembagian IP) <br>
- Menentukan subnet yang ada dalam topologi dan lakukan labelling netmask terhadap masing-masing subnet kemudian menggabungkan subnet-subnet sampai menjadi subnet besar. <br>
![CIDR Topologi](https://user-images.githubusercontent.com/61286109/102782210-8c86d200-43cb-11eb-8316-12f8969fb196.png) <br>
Hasil yang didapat adalah <b>Netmask /22</b> untuk subnet besar topologi diatas.
- Pembagian IP dengan pohon berdasarkan penggabungan subnet yang telah dilakukan.
![Tree CIDR Modul5](https://user-images.githubusercontent.com/61286109/102784642-43388180-43cf-11eb-8c7a-9bc348e96a5f.png) <br>
- Dari pohon tersebut akan mendapat pembagian IP sebagai berikut. <br>
![Pembagian IP](https://user-images.githubusercontent.com/61286109/102786622-657fce80-43d2-11eb-9fcc-d6e9866b04ca.PNG)) <br>

#### Konfigurasi Tiap UML
- Pertama, membuat file `topologi.sh`.
![NanoTopologi](https://user-images.githubusercontent.com/61286109/102784082-73cbeb80-43ce-11eb-89e5-5bfb656220a9.PNG) <br>
