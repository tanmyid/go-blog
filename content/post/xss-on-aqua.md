---
title: "Cross Site Scripting on Aqua.co.id"
author: "Tan"
tags: ["XSS", "WriteUP", "Web"]
date: 2019-08-24T01:51:50+07:00
draft: false
---

Berjumpa lagi, masih bermain main dengan XSS, kali ini saya mendapat sebuah website bernama Aqua, ya pasti kalian pada tahu dong, Aqua adalah sebuah anak perusahaan dari `Danone` yang berjalan pada sentra air minum.

[![logo-aqua](https://i.postimg.cc/pVg1bGqc/image.png)](https://postimg.cc/QVJqcbLc)

Kebetulan saya bekerja sebagai admin di sebuah CV yang bekerja sama dengan Aqua sebagai mitra utamanya. Kalian bisa mengunjungi website [aqua.co.id](https://aqua.co.id/) untuk mendapatkan informasi lebih lanjut tentang perusahaan ini.

*Lho kok ga di laporkan aja lur?* Intinya saya males **mereport** bug seperti ini, endingnya hanya di anggap remeh saja sama mereka, kan kata **mereka** XSS hanya hal sepele hihi, jadi males aja gitu lur. Mending di WriteUP dan untuk saling share pengalaman.

Oke menuju topik utama, setelah iseng mengunjungi halaman [aqua.co.id](https://aqua.co.id) saya mendapatkan from pencarian, nah  disitu saya mendapat ide untuk melakukan XSS.

[![From Pencarian di aqua.co.id](https://i.postimg.cc/3JTCFPLh/image.png)](https://postimg.cc/SjgMytF1)

Saya mencoba memasukkan `"><script>alert(1337);</script>` tetapi yang muncul halaman berikut, awalnya saya merasa gagal.

[![gagal](https://i.postimg.cc/Dwsh34rL/image.png)](https://postimg.cc/Wd2CM3f1)

Lalu mencoba meremove *search* pada url dari :
```
https://aqua.co.id/search?_token=Pt6XdGZA2MyJqSY3RuT33O9ThJB0svRVqtiYE3Dn&query=%22%3E%3Cscript%3Ealert%281337%29%3B%3C%2Fscript%3E
```
menjadi
```
https://aqua.co.id/?_token=Pt6XdGZA2MyJqSY3RuT33O9ThJB0svRVqtiYE3Dn&query=%22%3E%3Cscript%3Ealert%281337%29%3B%3C%2Fscript%3E
```
Dan boom akhirnya muncul juga popup **1337** yang saya dambakan, cieeee....

[![1337](https://i.postimg.cc/9fns4WYC/image.png)](https://postimg.cc/3y2tVQqc)

Tak lupa mencoba memunculkan **prompt** agar terdokumentasi bagi saya hehe.

[![XSS on Aqua.co.id](https://i.postimg.cc/XqZKDbRN/XSS-aqua.png)](https://postimg.cc/fSN0J6W1)

Iseng lagi coba coba ternyata bisa render html juga.

[![SC](https://i.postimg.cc/zfgFj81J/image.png)](https://postimg.cc/7CqzPv6c)

Sekian writeup dari saya, jika ada kesalahan kata saya minta maaf. Terima kasih