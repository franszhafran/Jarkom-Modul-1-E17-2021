## Nomor 1
_1. Sebutkan web server yang digunakan pada "ichimarumaru.tech"!_

Untuk mencari web server, terdapat pada HTTP header. Oleh karena itu, kita dapat memfilter semua paket yang berisi ichimarumaru.tech<br />
![image](https://user-images.githubusercontent.com/49693862/134770746-fa8ae1e9-0569-4f3a-91bb-e4b9a05ab5bd.png)<br />
Selanjutnya, pilih paket kemudia follow HTTP stream<br />
![image](https://user-images.githubusercontent.com/49693862/134770760-9e8ad384-85e8-40e6-9345-b5982c306856.png)<br />
Dapat dilihat ichimarumaru.tech menggunakan web server nginx

## Nomor 2
_2.Temukan paket dari web-web yang menggunakan basic authentication method!_

Untuk mencari paket dengan basic authentication method, dapat menggunakan display picture http.authbasic<br />
![image](https://user-images.githubusercontent.com/49693862/134770800-8d6b02ee-e869-4172-a21f-faa4bcb6c761.png)<br />
Didapatkan paket-paket yang menggunakan basic authentication method

## Nomor 3
_3. Ikuti perintah di basic.ichimarumaru.tech! Username dan password bisa didapatkan dari file .pcapng!_

Setelah mendapatkan paket yang menggunakan basic authentication, di headernya terdapat username dan password untuk basic.ichimarumaru.tech<br />
![image](https://user-images.githubusercontent.com/49693862/134770846-a1b1580a-227f-4fd8-b163-53c2bee515b7.png)<br />
Di web basic.ichimarumaru.tech diminta urutan konfiguarasi pengkabelan T568A, dengan urutan terlampir pada gambar

## Nomor 4
_4.Temukan paket mysql yang mengandung perintah query select!_

Untuk mencari diantara paket mysql dapat menggunakan filter mysql, kemudian karena ingin mencari paket dengan perintah select gunakan contains select<br />
![image](https://user-images.githubusercontent.com/49693862/134771206-4e6fa6e2-fd41-4754-8156-5a1b41d89956.png)<br />

Dengan filter tersebut, perintah select dengan kapital tidak akan tertangkap, maka bisa menggunakan matches dengan regex **mysql matches "select"** <br />
![image](https://user-images.githubusercontent.com/49693862/134771278-42acf1d1-1b2f-4396-8b10-68ad232920f0.png)<br />

## Nomor 5
_5.Login ke portal.ichimarumaru.tech kemudian ikuti perintahnya! Username dan password bisa didapat dari query insert pada table users dari file .pcap!__

Pertama, cari terlebih dahulu paket mysql yang mengandung insert dengan **mysql matches "insert"** <br />
![image](https://user-images.githubusercontent.com/49693862/134771315-37940d47-d3bc-4710-80db-41be8c2f6181.png)<br />

Pada frame terdapat query insert nya, didapatkan username dan password, lalu ke portal.ichimarumaru.tech. Ternyata diminta urutan konfigurasi pengkabelan T568B, dengan urutan seperti pada gambar<br />
![image](https://user-images.githubusercontent.com/49693862/134771344-54a9d74e-cea3-4295-b388-a6b623038c91.png)<br />

Kendala: awalnya berpikir bahwa passwordnya harus di MD5 terlebih dahulu, ternyata bisa langsung menggunakan yang tertera pada query insert.


## Nomor 6
_6.Cari username dan password ketika melakukan login ke FTP Server!_

Pertama, cari paket dengan protokol ftp dengan display filter ftp. Kemudian terlihat urutan paket, untuk username terdapat pada paket dengan USER. Untuk password terdapat paket dengan PASS.<br />
![image](https://user-images.githubusercontent.com/49693862/134771370-ac7bda12-ddcd-4348-b079-214bd6b4c065.png)<br />

## Nomor 7
_7.Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")_

Pertama filter paket dimana pada frame terdapat Real.pdf, kemudian didapat dari ftp-data pada file 201.zip terdapat file Real.pdf. Follow TCP stream paket tersebut kemudian set as raw lalu download sebagai Real.pdf. Isi dari real pdf adalah sebagai berikut<br />
![image](https://user-images.githubusercontent.com/49693862/134771662-9fed77ce-c760-4676-be12-59fcce22c223.png)<br />

Kendala: awalnya berpikir tidak bisa menggunakan frame, karena data yang dikirim compressed sebagai zip. Namun setelah dicoba ternyata berhasil menemukan paket yang mengandung Real.pdf

## Nomor 8
_8.Cari paket yang menunjukan pengambilan file dari FTP tersebut!_

Untuk mencari paket yang mengambil file, gunakan display filter **ftp.request.command  == "RETR"** <br />
![image](https://user-images.githubusercontent.com/49693862/134771706-34ebcd2c-0c41-426e-be95-34fa00aa1fc7.png)<br />

Kendala: berpikir bahwa salah karena hasil tidak ada, ternyata memang dari .pcapng nya tidak ada paket mengambil file.

## Nomor 9
_9.Dari paket-paket yang menuju FTP terdapat inidkasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!_

Untuk mencari paket ftp dengan nama file secret.zip, gunakan display filter **ftp-data** kemudian didapatkan paket secret.zip. Follow TCP Stream kemudian set as raw kemudian save sebagai secret.zip. <br />
![image](https://user-images.githubusercontent.com/49693862/134771749-52b9d0ce-2999-4fcf-b40d-ed61a4dd78ff.png)<br />

Isi dari secret.zip adalah Wanted.pdf <br />
![image](https://user-images.githubusercontent.com/49693862/134771755-1c292f7d-e0ec-4d26-af62-c1364aef961b.png)<br />

## Nomor 10
_10.Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!_

Cari di ftp-data dengan nama file history.txt. Isi dari history txt adalah sebagai berikut
> ls<br />
> key="$(tail -1 bukanapaapa.txt)"<br />
> zip -P $key secret.zip Wanted.pdf<br />
> rm Wanted.pdf<br />
> history | tail -5 > history.txt<br />

Bisa dilihat, secret.zip menggunakan password dari variable key, yang merupakan output dari command **tail -1 bukanapaapa.txt** . Oleh karena itu, cari dulu file bukanapapa.txt dengan menulusuri ftp-data dan mencari file bukanapaapa.txt. Setelah didapatkan kemudian follow TCP stream dan set raw lalu save menjadi bukanapaapa.txt. Lalu jalankan **tail -1 bukanapaapa.txt**. <br />
![image](https://user-images.githubusercontent.com/49693862/134771927-975c4157-9fbf-448b-97c1-6648197d76b5.png)<br />
Didapatkan outputnya adalah d1b1langbukanapaapajugagapercaya yang berarti adalah password dari secret.zip. Kemudian buka Wanted.pdf dengan password tersebut, didapatkan isi dari Wanted.pdf adalah poster One Piece.<br />
![image](https://user-images.githubusercontent.com/49693862/134771957-64142156-c512-42c5-8399-b1ed8edeb4d4.png)<br />

> Nomor 11-15 menggunakan capture filter

## Nomor 11
_11.Filter sehingga wireshark hanya mengambil paket yang berasal dari port 80!_

Menggunakan capture filter **src port 80** . Berikut adalah hasilnya bila kita mengakses web http.<br />
![image](https://user-images.githubusercontent.com/49693862/134772003-7c919c86-4581-4a8e-a2fb-0fccb9e939b2.png)<br />

## Nomor 12
_12.Filter sehingga wireshark hanya mengambil paket yang mengandung port 21!_

Menggunakan capture filter **port 21** . Berikut adalah hasilnya bila kita mengakses ftp. Bila kita mengakses ftp pada lokal, ganti interface ke Loopback karena akan mengakses IP kita.<br />
![image](https://user-images.githubusercontent.com/49693862/134772017-1b6ff771-59ca-4a2a-a0f7-f8cd8bbabfc9.png)<br />

Kendala: lupa menggunakan loopback interface

## Nomor 13   
_13.Filter sehingga wireshark hanya menampilkan paket yang menuju port 443!_

Menggunakan capture filter **dst port 443** . Berikut adalah hasilnya bila kita mengakses web https.<br />
![image](https://user-images.githubusercontent.com/49693862/134772042-fd4a68ae-d616-4cc9-8ba5-ffe60e16056a.png)<br />

## Nomor 14
_14.Filter sehingga wireshark hanya mengambil paket yang tujuannya ke kemenag.go.id!_

Menggunakan capture filter **dst kemenag.go.id** . Berikut adalah hasilnya bila kita mengakses web kemenag.go.id<br />
![image](https://user-images.githubusercontent.com/49693862/134772054-73f0622d-7341-4b1d-a39a-b7638b1b1a99.png)<br />

## Nomor 15
_15.Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!_

Cek ip dengan ifconfig, sesuaikan dengan interface. Kemudian gunakan capture filter **src 192.168.1.109**<br />
![image](https://user-images.githubusercontent.com/49693862/134772076-2f67ed9d-f809-4246-8b12-813805bbcfdc.png)<br />
