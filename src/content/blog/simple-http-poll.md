---
title: 'HTTP Polling dengan Multi-Tab Coordination'
description: 'Library vanilla JavaScript untuk HTTP polling dengan koordinasi antar tab browser'
pubDate: 'Jan 28 2026'
heroImage: '/simple-http-poll.png'
---

#### HTTP Polling Itu Apa Sih?
Jadi gini, HTTP Polling itu teknik di mana browser kamu terus-terusan nanya ke server "eh ada data baru nggak?". Simpel kan? Teknik ini sering dipakai kalau butuh update berkala tapi males setup WebSocket yang ribet.

Nah masalahnya, kalau user buka banyak tab sekaligus, tiap tab bakal nge-poll sendiri-sendiri. Bayangin aja, buka 5 tab = 5x request ke server. Server kamu bisa nangis kejer kalau traffic-nya gede.

#### Kenalan Sama Simple HTTP Poll
Nah, buat solve masalah itu, gue bikin [Simple HTTP Poll](https://github.com/hrydi/simple-http-poll) - library vanilla JavaScript yang punya fitur keren: **multi-tab coordination**. Jadi cuma satu tab doang yang nge-poll, tab lainnya tinggal terima beres data yang udah di-sync. Hemat banget kan?

#### Fitur-Fitur Keren
Ini dia yang bikin library ini worth it:
1. ```Multi-Tab Coordination```: Cuma tab leader yang kerja, tab lain santai nerima data aja.
2. ```Automatic Leader Election```: Tab leader ditutup? Tenang, tab lain otomatis naik jabatan jadi leader baru.
3. ```Memory Safe```: Nggak ada memory leak, cleanup-nya bener.
4. ```State Sync```: Start/stop polling ke-sync di semua tab.
5. ```AbortController Support```: Bisa cancel request dengan aman.
6. ```Framework-Agnostic```: Pure JavaScript, mau pakai React, Vue, atau vanilla sekalipun bisa.

#### Cara Pakai
Clone dulu dari repo:
```bash
git clone https://github.com/hrydi/simple-http-poll.git
```

Terus tinggal pakai deh:
```javascript
const poller = new HTTPPolling({
  url: 'https://api.example.com/data',
  interval: 5000 // poll tiap 5 detik
});

// Terima data
poller.onData((data) => {
  console.log('Dapat data nih:', data);
});

// Gas polling!
poller.startPolling();
```

#### Handle Error
Kalau ada yang error, tinggal tangkep aja:
```javascript
poller.onError((error) => {
  console.error('Waduh error:', error.message);
});
```

#### Tau Kapan Jadi Leader
Pengen tau kapan tab kamu jadi leader atau follower? Gampang:
```javascript
poller.onLeaderChange((isLeader) => {
  if (isLeader) {
    console.log('Yeay, tab ini jadi boss sekarang!');
  } else {
    console.log('Yaudah jadi follower aja deh.');
  }
});
```

#### Method Lain yang Berguna
* ```stopPolling()```: Berenti polling dulu.
* ```isLeader()```: Ngecek tab ini leader atau bukan.
* ```configure(options)```: Ubah setting pas lagi jalan.
* ```getLastData()```: Ambil data terakhir yang ke-cache.
* ```destroy()```: Bersihin semuanya, panggil ini pas component unmount.

#### Gimana Cara Kerjanya?
Library ini pakai **BroadcastChannel API** buat ngobrol antar tab. Kalau browser nggak support, dia fallback ke **localStorage**. Alurnya gini:

1. Pas polling mulai, semua tab rebutan jadi leader.
2. Yang menang jadi leader, dia yang nge-request ke server.
3. Hasilnya di-broadcast ke semua tab follower.
4. Leader-nya tutup tab? Tab lain otomatis gantiin.

Simple kan?

#### Kapan Harus Pakai Ini?
Library ini cocok banget buat:
- Dashboard yang butuh refresh data berkala
- Notifikasi simpel tanpa WebSocket
- Monitoring status app
- Fitur yang butuh data fresh tapi nggak mau ribet

#### Penutup
Intinya, Simple HTTP Poll ini bikin hidup lebih gampang kalau kamu butuh polling tapi nggak mau bikin server kewalahan gara-gara multi-tab. Satu tab kerja, yang lain santai - efisien!

Cek langsung repo-nya di [GitHub](https://github.com/hrydi/simple-http-poll) ya. Feel free buat contribute atau kasih feedback!
