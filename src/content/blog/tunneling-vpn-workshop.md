---
title: 'Gara-Gara IP Conflict, Kepaksa Belajar Tunneling Dadakan Pas Workshop'
description: 'Cerita random pas 2 hari ikut workshop diluar kantor, VPN nyambung tapi SSH nggak bisa, ujung-ujungnya nyasar ke dunia tunneling dan SOCKS5'
pubDate: 'Jul 19 2026'
---

#### Ceritanya Gini
Jadi 2 hari aku nggak dikantor, disuruh ikut workshop diluar. Namanya kerja ya tetep jalan, jadi butuh remote ke server juga dong. Ritual biasa: akses VPN, terus lanjut mengabdi deh.

Tapi ternyata jalan hidup nggak semulus keju oles. Akses server production lancar jaya via browser, tapi begitu coba SSH, gagal mulu. Duh, butuh SSH lagi soalnya.

Masuk akal sih kalau via browser bisa akses server production, soalnya mereka ada dibelakang proxy. Otomatis network-nya beda lagi. Nah kalau buat SSH, ada yang bilang mungkin diblokir sama yang manage network gedung tempat workshop. Entahlah, aku mah cuma mau lanjut mengabdi aja hahahaha.

#### Ternyata Bukan Diblokir, Tapi IP Conflict
Ngeluh dong ke team yang ngurusin VPN. Udah berdebat panjang kali tinggi selama hari pertama, bengong aja nggak nemu solusi. Eh di hari ke-2 baru ngeh, ternyata IP address yang didapet dari AP tempat workshop **konflik** sama IP server tujuan. Kata yang ngurusin VPN gitu.

Yah, maklum lah, skill jaringan cuma 0.9. Tapi 0.9 itu cuma angka, jangan berkecil hati, hahahaha berusaha menyenangkan hati sendiri.

Kalau kalian terus berusaha, insha Allah dikasih kemudahan. Jangan lupa dibarengi doa 😀 (sisip kajian dikit ah, kata orang "sampaikan walau sedikit" hahahaha)

#### Inget-Inget Ilmu Tunneling Zaman Dulu
Dikasih inget lagi sama memory zaman dulu, ada yang namanya tunneling. Jangan tanya detail teknisnya kayak gimana, aku nggak paham-paham amat, modal baca `--help` doang hahahaha.

Terus apa hubungannya sama masalah IP conflict ini? Nah, mikirnya gini: kalau IP-nya konflik, ya udah pake IP lain aja. Jadi ngobrol lagi sama yang manage server, minta akses ke server selain IP A tadi.

Nggak pake lama, sat set sat set, nemu satu server yang bisa diakses, sebut aja IP B. Tapi ya nggak mulus-mulus amat kek keju oles juga, server-nya diprotect pake OTP. Duh, sikuriti nya tolong hahahaha.

#### Masalahnya, Akses OTP Juga Dibelakang Network yang Sama
Nah ini yang bikin muter otak. Akses buat generate OTP itu harus lewat browser yang ada dibelakang network server yang justru nggak bisa aku akses. Bingung nggak baca ini? Sama, aku juga bingung jalaninnya hahahaha.

Solusinya? Social engineering dikit lah. Japri team yang available dikantor, minta tolong ambilin OTP-nya hahaha.

#### Akhirnya Tunnel Berhasil
Alhamdulillah, tunnel-nya jalan. Lanjut setup browser via SOCKS5, terus setup SSH juga lewat SOCKS5 yang sama. Lumayan, masih sisa waktu sejam di workshop sebelum balik hahahaha.

#### Penutup
Intinya, gara-gara satu masalah IP conflict, jadi keingetan lagi sama konsep tunneling yang udah lama nggak disentuh. Kadang solusi teknis itu nggak harus canggih-canggih amat, yang penting nyambungin titik-titik yang ada: SOCKS5, SSH, sama sedikit bantuan orang baik dikantor.

Terima kasih buat semua yang terlibat bantu selesain masalah ini, kalo bisa kucium satu-satu deh kalian 😗
