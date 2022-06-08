<h1 align="center">Tugas Akhir Arsitektur Jaringan Terkini</h1>
Disini saya akan menjalaskan 4 bagian utama dari tugas akhir matakuliah arsitekur jaringan terkini.
<hr>

## Bagian 1

### Membuat EC2 Instance di AWS Academy:
- Name and tags: Tugas Akhir
<p align="center"> <img src="picture/Screenshot_1.png" style="width:80%"/>

- OS Images: Ubuntu Server 22.04 LTS 64 bit
<p align="center"> <img src="picture/Screenshot_2.png" style="width:80%"/>

- Instance type: t2.medium
<p align="center"> <img src="picture/Screenshot_3.png" style="width:80%"/>

- Key pair: vockey
<p align="center"> <img src="picture/Screenshot_4.png" style="width:80%"/>

- Edit Network settings: allow SSH, allow HTTP, allow HTTPS, allow TCP port 8080, allow TCP port 8081
<p align="center"> <img src="picture/Screenshot_5.png" style="width:80%"/>

- Configure storage: 30 GiB, gp3
<p align="center"> <img src="picture/Screenshot_6.png" style="width:80%"/>

- Menyiapkan instalasi Mininet+OpenFlow, Ryu dll setelah ec2 telah siap dan telah tersambung dengan ssh.Dengan rujukan yang saya gunakan sebagai berikut [instrutur instalasi](https://awsacademy.instructure.com/courses/15355/discussion_topics/32566)
- Pertama kali kita harus melakukan update dan upgrade dengan perintah `sudo apt -yy update && sudo apt -yy upgrade`
- Lalu saya akan memulai instalasi Mininet + OpenFlow dengan mengunduh repositori Mininet dengan perintah `git clone https://github.com/mininet/mininet`.Dilanjutkan dengan perintah untuk proses instalasinya `mininet/util/install.sh -nfv`
- Berikutnya saya melakukan intalasi Ryu dengan menggunakan perintah 

    `git clone https://github.com/osrg/ryu.git`

    `cd ryu; pip install .`

    `cd`
- Terakhir Instalasi FlowManager dengan perrintah `git clone https://github.com/martimy/flowmanager`
- Jika semua telah berhasil akan menghasilkan direktori pada home sebagai berikut 
<p align="center"> <img src="picture/Screenshot_7.png" style="width:80%"/>
<hr>

## Bagian 2 
### Membuat custom Topology Mininet seperti gambar dibawah:
<p align="center"> <img src="picture/Screenshot_12.png" style="width:80%"/>
</p>

- Pertama kita membuat custom topologi mininet bertipe file python disini menggunakan file topo.py
- kemudian kodingan python tersebut kita jalankan menggunakan mininet dengan perintah `sudo mn --controller=none --custom topo.py --topo mytopo --mac --arp`. Jika telah berhasil dijalankan tampilan akan sebagai berikut.
<p align="center"> <img src="picture/Screenshot_8.png" style="width:80%"/>

- kemudian pada mininet kita melakukan testing apakah switch dan host telah  terhubung dengan baik dengan perintah `links` berikut tampilan jika semuanya telah terhubung.
<p align="center"> <img src="picture/Screenshot_9.png" style="width:80%"/>

- kali ini kita tidak dapat melakukan ping antar host dikarenakan belum adanya flow. selanjutnya kita akan membuat flow agar host dapat berhubungan satu dengan lainnya dengan perintah pada mininet yanga da di file Perintah_flow.txt. Jika telah berhasil tampilan akan sebagai berikut: 
<p align="center"> <img src="picture/Screenshot_10.png" style="width:800%"/>

- lalu kita coba test ping semuanya dengan perrintah yang ada dimininet yaitu `pingall`. Jika telah sukses akan terlihat seperti gambar dibawah tidak ada dara yang droped atau semunya telah menerima data.
<p align="center"> <img src="picture/Screenshot_11.png" style="width:80%"/>

- kemudian kita coba test ping ping dengan manual dengan menentukan jalur yang di inginkan disini saya mencoba mwlakukan testing h1 ke h2 dan h5 ke h6. dan hasilnya seperti gambar di bawah.
<p align="center"> <img src="picture/Screenshot_13.png" style="width:80%"/>
<p align="center"> <img src="picture/Screenshot_14.png" style="width:80%"/>

## Bagian 3

### Membuat Aplikasi Ryu Load Balancer 
- Disini saya memodifikasi code program load balancer sesuai yang di minta ditugas memiliki 4 host dan host pertama nantinya dijadikan sebagai client web. Dan diseting dengan algoritma round-robin untuk dapat berganti ganti dengan server web dari h1-h4.<p align="center"> <img src="picture/Screenshot_15.png" style="width:80%"/>

- Disini menggunakan mininet untuk mebuat topologi sesuai yang ada di soal topo single dan memiliki 4 host menggunakana kontroler remote switch ovs dengan protocol openflow13. Dan jika berhasil dibuat. Kita membuat h2,h3,h4 untuk dijadikan sebagai web server dengan perintah mininet h(n) python3 -m http.server 80 &.<p align="center"> <img src="picture/Screenshot_16.png" style="width:80%"/>

- Kemudian dengan h1 curl 10.0.0.100 host 1 akan menjadi client web<p align="center"> <img src="picture/Screenshot_17.png" style="width:80%"/>

- Jika telah berhasil kita jalankan program dengan perintah ryu run llb.py dan h1 sebagai client web akan bergantian mengirimkan paket ke server web seperti gambar dibawah<p align="center"> <img src="picture/Screenshot_18.png" style="width:80%"/>
<hr>

## Bagian 4

### membuat Aplikasi Ryu  Shortest Path Routing

- Pertama kali yang harus kita lakukan adalah clone sdn dari github dengan perintah `git clone https://github.com/abazh/learn_sdn`.<p align="center"> <img src="picture/Screenshot_19.png" style="width:80%"/>
- Kedua menunjuk directory learn_sdn/Spf dengan perintah `cd learn_sdn/Spf`<p align="center"> <img src="picture/Screenshot_20.png" style="width:80%"/>
- Ketiga menjalankan controller ryu dengan perintah `ryu-manager --observe-links dijkstra_Ryu_controller.py` <p align="center"> <img src="picture/Screenshot_21.png" style="width:80%"/>
- Keempat menjalankan topologi mininet dengan perintah `sudo python3 topo-spf_lab.py`<p align="center"> <img src="picture/Screenshot_22.png" style="width:6100%"/>
- Sekarang melakukan uji coba pada koneksi h1 ke h6 dengan perintah `hi ping -c4 h6`. H1 dapat bisa melakukan ping terhadap h6. Pada saat pertamakali ping lebih tinggi dibandingkan ping berikutnya karena pada ping tersebut menentukan rute terpendek untuk mencapai host tujuan seperti gambar dibawah<p align="center"> <img src="picture/Screenshot_23.png" style="width:80%"/>
- Dan Controller ryu menampilkan pencarian pada switch tujuan terkoneksi, masing masing merespon paket tersebut.<p align="center"> <img src="picture/Screenshot_24.png" style="width:80%"/>
- Controller melakukan pencarian rute tercepat dan rute yang dilalui dari h6 ke h1 adalah s6, s3, dan s1. Setelah itu, flow akan diberikan ke switch yang terkait dengan rute<p align="center"> <img src="picture/Screenshot_25.png" style="width:80%"/>
- Ping berikutnya tanpa melakukan pencarian rute lagi langsung dikirimkan ke rute sebelumnya.<p align="center"> <img src="picture/Screenshot_26.png" style="width:80%"/>
- H1 melakukan ping ke h3 . lalu ditemukan 2 rute alternative yang sama yang menyebabkan packet looping.<p align="center"> <img src="picture/Screenshot_27.png" style="width:80%"/>
- Rute yang dilalui adalah s1,s2,dan s4<p align="center"> <img src="picture/Screenshot_28.png" style="width:80%"/>

