# C4_Modul1

*Ada repositori bot line dengan python pada akun ini*

##  No. 1

1.  Anda diminta tolong oleh teman anda untuk mengembalikan filenya yang telah dienkripsi oleh seseorang menggunakan bash script, file yang dimaksud adalah nature.zip. Karena terlalu mudah kalian memberikan syarat akan membuka seluruh file tersebut jika pukul 14:14 pada tanggal 14 Februari atau hari tersebut adalah hari jumat pada bulan Februari. Hint: Base64, Hexdump
```
//BASH SCRIPT
#!/bin/bash
iter=0    
for gambarnya in /home/andhika/sisop19/modul1/nature/*.jpg
do
base64 -d $gambarnya | xxd -r > /home/andhika/sisop19/modul1/gambar_kebuka/$iter.jpg
iter=$((iter + 1))
done
```
PENJELASAN
- Pertama, extract isi nature.zip ke direktori yang diinginkan, disini kami menggunakan direktori /home/andhika/sisop19/modul1/nature/.

- Selanjutnya dalam bash script yang akan dijalankan, buat sebuah variable untuk penamaan file berdasarkan variable tersebut, pada script ini digunakan variable ITER dengan value awal 0 dan akan terus bertambah sampai loop selesai.

```for gambarnya in /home/andhika/sisop19/modul1/nature/*.jpg```
- variable gambarnya akan menyimpan file *.jpg selama file jpg di folder tersebut masih ada.

```base64 -d $gambarnya```
- akan men-decode base64 pada $gambarnya, xxd -r akan mengconvert hexdump ke binary. iter=$((iter + 1)) akan meng-increment iter.
```
//CRONTAB
14 14 14 2 5 /bin/bash /home/andhika/sisop19/modul1/soal1.sh
```
- Kemudian, buka crontab -e dan masukkan 14 14 14 2 5 /bin/bash /home/andhika/sisop19/modul1/soal1.sh dengan tujuan agar pada pukul 14:14 pada tanggal 14 Februari atau pada hari jumat bulan februari, script soal1.sh akan di-bash.

##  No. 2

2.  Anda merupakan pegawai magang pada sebuah perusahaan retail, dan anda diminta untuk memberikan laporan berdasarkan file WA_Sales_Products_2012-14.csv. Laporan yang diminta berupa:

    a.  Pentukan negara dengan penjualan(quantity) terbanyak pada tahun 2012.

    b.	Tentukan tiga product line yang memberikan penjualan(quantity) terbanyak pada soal poin a.

    c.  Tentukan tiga product yang memberikan penjualan(quantity) terbanyak berdasarkan tiga product line yang didapatkan pada soal poin b

###### 2.a.

```
awk -F, '{if($7=='2012') iter [$1]+=$10} END{for (hasil in iter) print iter[hasil], hasil}' WA_Sales_Products_2012-14.csv OFS=',' | sort -nr | head -n1 | awk -F, '{print $2}'
```
######  Penjelasan:

- awk -F, adalah perintah awk dengan menggunakan field separator ‘,’ (koma), selanjutnya if($7=='2012') iter [$1]+=$10  apabila field ke-7 (year) bernilai 2012 maka array dengan setiap nama negara akan menyimpan jumlah dari field ke-10 (quantity). for (hasil in iter) print iter[hasil], hasil  akan melakukan print quantity dan nama negara. OFS=','  disini digunakan untuk output field separator berupa koma. sort -nr akan mengurutkan numeric reverse yaitu dari quantity yang terbesar ke terkeci. head -n1 digunakan untuk mengambil baris pertama yaitu baris dengan negara yang memiliki quantity terbanyak. Dari hasil sebelumnya, tercetak dengan format Quantity,Nama_Negara, karena yang diinginkan adalah nama negaranya saja maka digunakan awk -F, '{print $2}' pada bagian akhir. Output yang dihasilkan berdasarkan script diatas yaitu “United States”.

###### 2.b.
```
awk -F, '{if($7 == '2012' && $1=="United States") iter[$4]+=$10} END {for(hasil in iter) {print iter[hasil], hasil}}' WA_Sales_Products_2012-14.csv OFS=',' | sort -nr | head -n3 | awk -F, '{print $2}'
```
######  Penjelasan:

- Mirip dengan 2a, if($7 == '2012' && $1=="United States") iter[$4]+=$10 dengan tambahan syarat yaitu negara yang diinginkan adalah “United States” (berdasarkan hasil dari 2a) dan menyimpan quantity dari product line pada array. for(hasil in iter) {print iter[hasil], hasil} digunakan untuk mencetak Quantity dan Product_Line. OFS=',' | sort -nr | head -n3 seperti sebelumnya, menggunakan output field separator berupa koma, mengurutkan dari terbesar ke terkecil dan mengambil 3 baris pertama yaitu baris yang memiliki quantity terbanyak dan product line-nya. Karena yang diinginkan hanya nama product line, digunakan perintah awk -F, '{print $2}'. Output yang dihasilkan adalah "Personal Accessories", "Camping Equipment", dan "Outdoor Protection"

###### 2.c.
```
awk -F, '{if(($7 == '2012' && $1=="United States")  && ($4 == "Personal Accessories" || $4 == "Camping Equipment" || $4 == "Outdoor Protection")) iter[$6]+=$10} END {for(hasil in iter) {print iter[hasil], hasil}}' WA_Sales_Products_2012-14.csv OFS=',' | sort -nr | head -n3 | awk -F, '{print $2}'
```

######  Penjelasan:

- Berdasarkan output yang dihasilkan pada 2b, tambahan pada if yaitu  ($4 == "Personal Accessories" || $4 == "Camping Equipment" || $4 == "Outdoor Protection"). iter[$6]+=$10 digunakan untuk menyimpan quantity dari product dalam array. for(hasil in iter) {print iter[hasil], hasil} untuk mencetak quantity dan productnya. OFS=',' | sort -nr | head -n3 seperti sebelumnya, menggunakan output field separator berupa koma, mengurutkan dari terbesar ke terkecil dan mengambil 3 baris pertama yaitu baris yang memiliki quantity terbanyak dan nama productnya. Karena yang diinginkan adalah nama product maka digunakan perintah awk -F, '{print $2}'. Output yang dihasilkan adalah “Zone”, “Trailchef Water Bag”, dan “Single Edge”.

##  No. 3

3.  Buatlah sebuah script bash yang dapat menghasilkan password secara acak sebanyak 12 karakter yang terdapat huruf besar, huruf kecil, dan angka. Password acak tersebut disimpan pada file berekstensi .txt dengan ketentuan pemberian nama sebagai berikut:

    a.	Jika tidak ditemukan file password1.txt maka password acak tersebut disimpan pada file bernama password1.txt

    b.	Jika file password1.txt sudah ada maka password acak baru akan disimpan pada file bernama password2.txt dan begitu seterusnya.

    c.	Urutan nama file tidak boleh ada yang terlewatkan meski filenya dihapus.

    d.	Password yang dihasilkan tidak boleh sama.


##  No. 4

4.  Lakukan backup file syslog setiap jam dengan format nama file “jam:menit tanggal-bulan-tahun”. Isi dari file backup terenkripsi dengan konversi huruf (string manipulation) yang disesuaikan dengan jam dilakukannya backup misalkan sebagai berikut:

    a.	Huruf b adalah alfabet kedua, sedangkan saat ini waktu menunjukkan pukul 12, sehingga huruf b diganti dengan huruf alfabet yang memiliki urutan ke 12+2 = 14.

    b.	Hasilnya huruf b menjadi huruf n karena huruf n adalah huruf ke empat belas, dan seterusnya.

    c.	setelah huruf z akan kembali ke huruf a

    d.	Backup file syslog setiap jam.

    e.	dan buatkan juga bash script untuk dekripsinya.

##  No. 5

5.  Buatlah sebuah script bash untuk menyimpan record dalam syslog yang memenuhi kriteria berikut:

    a.	Tidak mengandung string “sudo”, tetapi mengandung string “cron”, serta buatlah pencarian stringnya tidak bersifat case sensitive, sehingga huruf kapital atau tidak, tidak menjadi masalah.

    b.	Jumlah field (number of field) pada baris tersebut berjumlah kurang dari 13.

    c.	Masukkan record tadi ke dalam file logs yang berada pada direktori /home/[user]/modul1.

d.	Jalankan script tadi setiap 6 menit dari menit ke 2 hingga 30, contoh 13:02, 13:08, 13:14, dst.
```
awk '/cron/ || /CRON/ && !/sudo/ && !/SUDO/' /var/log/syslog | awk 'NF<=12' >> /home/andhika/modul1/no_5_sisop.log
```
###### 5.a.

- Untuk menampilkan string “cron” yang tidak bersifat case sensitive dengan mengunakan perintah ```/cron/ || /CRON/``` sedangkan untuk menampilkan string yang tidak mengandung kata “sudo” dengan menggunakan perintah ```!/sudo/```

###### 5.b.

```NF<=12”``` 
- Untuk menampilkan jumlah field pada baris yang berjumlah kurang dari 13

###### 5.c.

```>> /home/andhika/modul1/no_5_sisop.log``` 
- Untuk memasukkan record tadi ke dalam file logs pada direktor /home/andhika/modul1/no_5_sisop.log

###### 5.d.

- Perintah pada nomor 5a, 5b, 5c disimpan pada file bernama sisop_no_5.sh pada direktori /home/andhika lalu edit crontab dengan mengetikkan “2-30/6 * * * * /bin/bash /home/andhika/sisop_no_5.sh” pada crontab lalu jalankan dengan mengetikkan crontab -l


