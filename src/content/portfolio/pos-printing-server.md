---
title: 'POS & Printing Server'
description: 'Aplikasi web-based Point of Sales dengan manajemen user, barang, stok & harga, tenant (cabang), warehouse, dan sistem silent print ESC/POS via sidecar printer server.'
pubDate: '2025-07-15'
heroImage: '/images/pos/sinarsiliwangi_pos_not_connected_printer_server.jpeg'
tags: ['PHP', 'jQuery', 'ESC/POS', 'Epson LX-310', 'MySQL', 'Electron']
repoUrl: 'https://github.com/hrydi/pos-printing-server'
liveUrl: '#'
---

**POS & Printing Server** adalah aplikasi Point of Sales berbasis web yang dirancang untuk menangani operasional bisnis skala kecil hingga menengah secara end-to-end.

## Fitur Utama

### Manajemen Bisnis Inti
- **User Management** — Role-based access control dengan level admin, kasir, dan owner.
- **Manajemen Barang & Stok** — CRUD barang, pencatatan stok real-time, serta riwayat mutasi stok antar warehouse.
- **Manajemen Harga** — Daftar harga fleksibel dengan support harga grosir, member, dan normal per item.
- **Tenant (Cabang) & Warehouse** — Multi-cabang dan multi-gudang dalam satu sistem. Setiap tenant memiliki stok dan harga tersendiri.

### Sistem Printing
- **Silent Print ESC/POS** — Print struk secara otomatis tanpa popup dialog printer, menggunakan raw ESC/POS command.
- **Epson LX-310 Support** — Target utama printer dot-matrix 9-pin untuk struk dan laporan.
- **Sidecar Printer Server** — Aplikasi client-side (desktop) yang dipasang di tiap PC kasir, bertindak sebagai jembatan antara web app dan printer lokal.

### Arsitektur Sidecar
Aplikasi web berkomunikasi dengan sidecar via HTTP API lokal (`localhost`). Sidecar menerima perintah cetak dalam format JSON, lalu mengirimkan raw ESC/POS bytes ke printer melalui port LPT/USB. Pendekatan ini memungkinkan:
- Print tanpa gangguan UI (tanpa popup print dialog browser).
- Dukungan untuk printer legacy berbasis ESC/POS.
- Keamanan — browser tidak perlu akses langsung ke printer.

## Tech Stack
- **Backend:** PHP
- **Frontend:** HTML + jQuery
- **Database:** MySQL
- **Printing:** Raw ESC/POS via Electron sidecar app

## Preview

![POS — Printer Server Not Connected](/images/pos/sinarsiliwangi_pos_not_connected_printer_server.jpeg)
![POS — Printer Server Connected](/images/pos/sinarsiliwangi_pos_connected_printer_server.jpeg)
![Printer Server App](/images/pos/sinarsiliwangi_printer_server.png)
