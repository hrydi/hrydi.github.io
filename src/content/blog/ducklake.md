---
title: 'DuckLake: Bikin Lakehouse Cuma Modal Extension DuckDB'
description: 'Kenalan sama DuckLake, extension DuckDB buat bikin data lakehouse tanpa ribet catalog server terpisah'
pubDate: 'Jul 6 2026'
heroImage: '/ducklake.svg'
---

#### DuckLake Itu Apa?
Kalau kamu udah kenalan sama [DuckDB](/blog/duckdb/), sekarang lanjut ke salah satu extension-nya yang lagi rame dibahas: **DuckLake**. Ini format lakehouse open-source yang dibikin sama tim DuckDB Labs, tujuannya buat nyaingin format-format lakehouse yang udah ada kayak Apache Iceberg atau Delta Lake.

Bedanya ada di pendekatan metadata-nya. Iceberg dan Delta Lake nyimpen metadata (schema, snapshot, manifest) dalam bentuk file JSON/Avro yang bertumpuk di storage. DuckLake justru naruh semua metadata itu di dalam **database SQL biasa** - bisa DuckDB sendiri, PostgreSQL, MySQL, atau SQLite. Data mentahnya tetep disimpen sebagai file Parquet di object storage kayak biasa.

#### Kenapa Pendekatan Ini Menarik?
Beberapa alasan kenapa DuckLake worth dilirik:
1. ```Metadata di SQL, Bukan Tumpukan File```: Nggak perlu scan ratusan file manifest cuma buat tau schema atau snapshot terakhir. Tinggal query SQL biasa.
2. ```Transaksi ACID Beneran```: Karena metadata-nya di database yang emang punya transaksi, concurrent write jadi lebih gampang dijamin konsistensinya.
3. ```Setup Simpel```: Nggak butuh catalog service terpisah kayak Hive Metastore atau Nessie. Modal DuckDB (atau Postgres) buat metadata udah cukup.
4. ```Kompatibel Sama Ekosistem Existing```: Data tetep Parquet, jadi tool lain yang ngerti Parquet tetep bisa baca file mentahnya.
5. ```Time Travel```: Sama kayak Iceberg/Delta, DuckLake juga dukung query ke versi data yang lama.

#### Cara Install Extension-nya
DuckLake dipasang sebagai extension DuckDB biasa. Buka DuckDB CLI atau dari koneksi Python, terus jalanin:
```sql
INSTALL ducklake;
LOAD ducklake;
```

#### Bikin Lakehouse Pertama Kamu
Contoh paling simpel, pakai DuckDB sendiri buat nyimpen metadata dan folder lokal buat data-nya:
```sql
INSTALL ducklake;
LOAD ducklake;

-- Attach lakehouse baru, metadata disimpen di metadata.ducklake
ATTACH 'ducklake:metadata.ducklake' AS gudang_data (DATA_PATH 'data_files/');

USE gudang_data;

CREATE TABLE penjualan (
    id INTEGER,
    produk VARCHAR,
    total DOUBLE
);

INSERT INTO penjualan VALUES
    (1, 'Kopi', 25000),
    (2, 'Teh', 15000);

SELECT * FROM penjualan;
```

Habis dijalanin, kamu bakal nemu file metadata `metadata.ducklake` dan folder `data_files/` yang isinya file Parquet asli. Simpel kan, nggak perlu setup catalog server terpisah.

#### Pakai PostgreSQL Buat Metadata
Kalau butuh setup yang lebih production-ready dengan banyak writer bersamaan, metadata-nya bisa dipindah ke PostgreSQL:
```sql
INSTALL postgres;
INSTALL ducklake;
LOAD ducklake;

ATTACH 'ducklake:postgres:dbname=lakehouse_meta host=localhost' AS gudang_data
    (DATA_PATH 's3://bucket-gue/data/');

USE gudang_data;
```

Jadi PostgreSQL-nya cuma nyimpen metadata (schema, snapshot, statistik), sedangkan data aslinya tetep di object storage kayak S3.

#### Time Travel, Ngecek Data Versi Lama
Setiap kali ada perubahan (insert/update/delete), DuckLake bikin snapshot baru. Kamu bisa query balik ke snapshot sebelumnya:
```sql
-- Lihat daftar snapshot yang ada
SELECT * FROM ducklake_snapshots('gudang_data');

-- Query data di snapshot tertentu
SELECT * FROM penjualan AT (VERSION => 1);
```

#### Kapan Cocok Pakai DuckLake?
DuckLake pas dipakai buat:
- Tim kecil-menengah yang mau bikin lakehouse tanpa ribet infra catalog service
- Proyek yang udah nyaman pakai DuckDB dan mau naik level ke penyimpanan data yang lebih terstruktur
- Kebutuhan analitik yang butuh time travel dan konsistensi ACID tapi nggak mau kompleksitas Iceberg/Delta penuh

Kalau kamu udah punya ekosistem besar berbasis Spark/Iceberg dengan banyak engine yang harus kompatibel, mungkin masih lebih pas pakai Iceberg. Tapi buat yang mau mulai simpel, DuckLake jadi jalan pintas yang masuk akal.

#### Penutup
DuckLake nunjukkin kalau bikin lakehouse nggak harus selalu ribet dengan banyak komponen infra. Modal DuckDB plus database SQL biasa buat metadata, kamu udah bisa punya data lake yang ACID-compliant dan support time travel. Worth dicoba kalau kamu lagi cari alternatif yang lebih ringan dari Iceberg atau Delta Lake.

Detail lengkapnya bisa dicek di [ducklake.select](https://ducklake.select).
