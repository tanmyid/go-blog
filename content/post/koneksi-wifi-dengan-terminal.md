---
title: "Mengkoneksikan Wifi Dengan Terminal Linux"
author: "Author Name"
tags: ["Linux", "Wireless", "tagC"]
date: 2019-05-15T18:03:32+07:00
draft: false
---

#### Pendahuluan
Terkadang jika kita memanajemen server tanpa GUI, kita di tuntut untuk mengoperasikan termilan, lalu bagaimana jika server tidak menggunakan LAN tapi menggunakan WiFi adapter? Nah pembahasan kali ini yaitu tentang cara simple mengkoneksinakan wifi di sistem operasi linux dengan terminal atau console.

Walau linux telah dilengkapi dengan network manager berbetuk GUI,namun untuk keadaan tertentu (perbaikan/linux server) kamu hanya bisa setting jaringan linux melalui terminal/shell.

#### Dependensi
Dependensi yang harus *dipenuhi* yaitu:

* iwconfig iwlist
* wpa_supplicant
* wpa_passphrase
* dhcpcd atau dhcpclient

#### Scan SSID
nah untuk scanning SSID kita bisa menggunakan `iwlist` dengan perintah:
``` 
root@ibislinux [ ~ ]# iwlist wlp2s0 scan | grep SSID
                      ESSID:"tethering"
                      ESSID:"PROGRAM"
                      ESSID:"New Office"
```
*note : wlp2s0 merupakan nama interface wireless saya, jadi kalian bisa mengganti dengan interface wireless masing masing*

Secara opsional, kalian bisa memunculkan data lengkap, misal kita ingin mengetahui data SSID dari `PROGRAM`
dengan perintah:
```
iwlist wlp2s0 scan essid "PROGRAM"
```
Hasilnya sebagai berikut, atau kalian bisa skip ke [koneksi tanpa password](#koneksi-tanpa-password) atau [koneksi menggunakan password](#koneksi-menggunakan-password)
```
root@ibislinux [ ~ ]# iwlist wlp2s0 scan essid "PROGRAM"
wlp2s0    Scan completed :
          Cell 01 - Address: 72:18:88:D8:4D:71
                    Channel:2
                    Frequency:2.417 GHz (Channel 2)
                    Quality=30/70  Signal level=-80 dBm  
                    Encryption key:on
                    ESSID:"tethering"
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 18 Mb/s
                              24 Mb/s; 36 Mb/s; 54 Mb/s
                    Bit Rates:6 Mb/s; 9 Mb/s; 12 Mb/s; 48 Mb/s
                    Mode:Master
                    Extra:tsf=00000003607fd030
                    Extra: Last beacon: 8040ms ago
                    IE: Unknown: 0009746574686572696E67
                    IE: Unknown: 010882848B962430486C
                    IE: Unknown: 030102
                    IE: Unknown: 0706236120010E1E
                    IE: Unknown: 2A0104
                    IE: Unknown: 2F0104
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: 32040C121860
                    IE: Unknown: 2D1A9E191BFFFF000001000000000000000000000000000000000000
                    IE: Unknown: 3D16020D1600000000000000000000000000000000000000
                    IE: Unknown: 4A0E14000A002C01C800140005001900
                    IE: Unknown: 7F080500000000000040
                    IE: Unknown: DD090010180200000C0000
                    IE: WPA Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: DD180050F2020101800003A4000027A4000042435E0062322F00
                    IE: Unknown: 46057200010000
          Cell 02 - Address: 00:19:E0:79:E9:3A
                    Channel:6
                    Frequency:2.437 GHz (Channel 6)
                    Quality=65/70  Signal level=-45 dBm  
                    Encryption key:on
                    ESSID:"PROGRAM"
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s
                              12 Mb/s; 24 Mb/s; 36 Mb/s
                    Bit Rates:9 Mb/s; 18 Mb/s; 48 Mb/s; 54 Mb/s
                    Mode:Master
                    Extra:tsf=0000000383c05b6b
                    Extra: Last beacon: 50ms ago
                    IE: Unknown: 000750524F4752414D
                    IE: Unknown: 010882848B960C183048
                    IE: Unknown: 030106
                    IE: Unknown: 0706494420010D14
                    IE: Unknown: 2A0100
                    IE: Unknown: 32041224606C
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : TKIP CCMP
                        Authentication Suites (1) : PSK
                       Preauthentication Supported
                    IE: WPA Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : TKIP CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: DD0900037F01010008FF7F
                    IE: Unknown: DD1A00037F03010000000019E079E93A0219E079E93A64002C010808
          Cell 03 - Address: F4:F2:6D:6F:32:E8
                    Channel:6
                    Frequency:2.437 GHz (Channel 6)
                    Quality=20/70  Signal level=-90 dBm  
                    Encryption key:on
                    ESSID:"TP-LINK_32E8"
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s
                              9 Mb/s; 12 Mb/s; 18 Mb/s
                    Bit Rates:24 Mb/s; 36 Mb/s; 48 Mb/s; 54 Mb/s
                    Mode:Master
                    Extra:tsf=00000025afcf146c
                    Extra: Last beacon: 1470ms ago
                    IE: Unknown: 000C54502D4C494E4B5F33324538
                    IE: Unknown: 010882848B960C121824
                    IE: Unknown: 030106
                    IE: Unknown: 050400010000
                    IE: Unknown: 0706555320010B1E
                    IE: Unknown: 2A0100
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: 32043048606C
                    IE: Unknown: 2D1AEF111BFFFF000000000000000000000100000000000000000000
                    IE: Unknown: 3D1606070100000000000000000000000000000000000000
                    IE: Unknown: 7F080000000000000040
                    IE: Unknown: DD180050F2020101800002A4400027A4000042435E0062322F00
                    IE: Unknown: DD0900037F01010000FF7F
                    IE: Unknown: DD260050F204104A0001101044000102104900140024E26002000101600000020001600100020001
          Cell 04 - Address: D4:6E:0E:3B:C8:22
                    Channel:13
                    Frequency:2.472 GHz (Channel 13)
                    Quality=22/70  Signal level=-88 dBm  
                    Encryption key:on
                    ESSID:""
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s
                              9 Mb/s; 12 Mb/s; 18 Mb/s
                    Bit Rates:24 Mb/s; 36 Mb/s; 48 Mb/s; 54 Mb/s
                    Mode:Master
                    Extra:tsf=000000016b2f0180
                    Extra: Last beacon: 140ms ago
                    IE: Unknown: 0000
                    IE: Unknown: 010882848B960C121824
                    IE: Unknown: 03010D
                    IE: Unknown: 050400010000
                    IE: Unknown: 0706494420010D14
                    IE: Unknown: 2A0100
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: 32043048606C
                    IE: Unknown: 2D1A6E1103FF00000000000000000000000000000000000000000000
                    IE: Unknown: 3D160D071100000000000000000000000000000000000000
                    IE: Unknown: DD180050F2020101810003A4000027A4000042435E0062322F00
                    IE: Unknown: DD0900037F01010000FF7F

```

#### Koneksi Tanpa Password
Kita menggunakan `iwconfig` untuk mengkoneksikan SSID yang tidak menggunakan password.
```
iwconfig wlp2s0 essid "PROGRAM"
```
Lalu request IP dari wireless dengan `dhcpcd`
```
dhcpcd wlp2s0
```
Cek cek apakah kita sudah mendapatkan IP dengan `ifconfig`
```

root@ibislinux [ ~ ]# ifconfig wlp2s0
wlp2s0    Link encap:Ethernet  HWaddr xx:xx:Xx:xx:xx
          inet addr:192.168.1.25  Bcast:192.168.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:75413 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6820 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:12439549  TX bytes:1132541

```
Sekian semoga bermanfaat.
#### Koneksi Menggunakan Password
Setelah mendapat target, kita *generate* file konfigurasi wireless nya yang berisi SSID dan password menggunakan `wpa_passphrase`, kalian bisa menaruh file nya di mana saja.
```
wpa_passphrase "PROGRAM" "12312345678" > ~/.config/wpa/program.conf
```
Berikut contoh isi file nya,
```
network={
	ssid="PROGRAM"
	#psk="12312345678"
	psk=e8bd99a04c19a9e5842cb9cc4c8883e2208134e369d2571bd3590ec63ef6913f
}
```

Selanjutnya yaitu mengkoneksikan komputer dengan wireless, dengan perintah,
```
root@ibislinux [ ~ ]# wpa_supplicant -c ~/.config/wpa/program.conf -i wlp2s0 -D wext -B
```
Sesuaikan nama file dan nama interface wireless kalian. Setelah sukses, kita request ip dari wifi dengan perintah
```
dhcpcd wlp2s0
```
Cek cek apakah kita sudah mendapatkan IP dengan `ifconfig`
```

root@ibislinux [ ~ ]# ifconfig wlp2s0
wlp2s0    Link encap:Ethernet  HWaddr xx:xx:Xx:xx:xx
          inet addr:192.168.1.25  Bcast:192.168.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:75413 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6820 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:12439549  TX bytes:1132541

```
Sekian semoga bermanfaat sekian dan terima kasih.