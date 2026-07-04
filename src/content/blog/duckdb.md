---
title: 'Kenalan Sama DuckDB, Database Analitik yang Ringan tapi Nendang'
description: 'Sekilas tentang DuckDB, database SQL in-process buat analisis data yang gampang banget dipakai'
pubDate: 'Jul 6 2026'
heroImage: '/duckdb.svg'
---

#### DuckDB Itu Apa Sih?
Jadi gini, DuckDB itu database SQL yang jalan **in-process**, alias nggak butuh server terpisah kayak PostgreSQL atau MySQL. Tinggal `import duckdb`, langsung bisa query. Sering disebut "SQLite-nya dunia analitik" karena filosofinya mirip: ringan, embedded, tanpa setup ribet.

Bedanya sama SQLite, DuckDB dioptimasi buat **OLAP** (analytical query) bukan OLTP (transaksi kecil-kecil). Jadi kalau kerjaan kamu banyak `GROUP BY`, `JOIN` gede, agregasi jutaan baris, di sinilah DuckDB unjuk gigi.

#### Kenapa Harus Coba DuckDB?
Beberapa alasan kenapa DuckDB makin populer di kalangan data engineer/analyst:
1. ```Zero Setup```: Nggak perlu install server, nggak perlu konfigurasi koneksi. Install library-nya doang, langsung gas.
2. ```Kenceng Buat Analitik```: Pakai columnar storage & vectorized execution, jadi query agregasi di data gede tetep ngebut.
3. ```Query Langsung ke File```: Bisa langsung `SELECT` dari file CSV, Parquet, atau JSON tanpa import dulu ke tabel.
4. ```Multi-Bahasa```: Ada binding buat Python, Node.js, Java, Go, Rust, R, sampai CLI standalone.
5. ```In-Memory atau Persisten```: Bisa jalan full in-memory buat eksperimen cepat, atau simpen ke file `.duckdb` kalau butuh persisten.

#### Cara Install
Paling gampang lewat pip kalau kamu pakai Python:
```bash
pip install duckdb
```

Atau kalau mau pakai CLI standalone:
```bash
curl https://install.duckdb.org | sh
```

#### Query Pertama
Contoh paling simpel, langsung query dari Python tanpa bikin tabel dulu:
```python
import duckdb

result = duckdb.sql("SELECT 42 AS jawaban_semesta").fetchall()
print(result)
```

#### Query Langsung dari File CSV/Parquet
Ini yang bikin DuckDB enak banget dipakai buat eksplorasi data cepat:
```python
import duckdb

# Langsung query file CSV, nggak perlu import manual
df = duckdb.sql("""
    SELECT kategori, COUNT(*) AS total
    FROM 'penjualan.csv'
    GROUP BY kategori
    ORDER BY total DESC
""").df()

print(df)
```

Mau baca file Parquet juga sama gampangnya, tinggal ganti nama file-nya aja. DuckDB otomatis ngerti format-nya dari ekstensi file.

#### Simpen Data ke File Persisten
Kalau nggak mau in-memory doang, tinggal kasih nama file pas connect:
```python
con = duckdb.connect('data_gue.duckdb')
con.execute("CREATE TABLE users (id INTEGER, nama VARCHAR)")
con.execute("INSERT INTO users VALUES (1, 'Budi'), (2, 'Ani')")

print(con.sql("SELECT * FROM users").fetchall())
```

Data-nya bakal ke-simpen di file `data_gue.duckdb`, jadi tetep ada meskipun program-nya udah selesai jalan.

#### Kapan Cocok Pakai DuckDB?
DuckDB cocok banget buat:
- Eksplorasi data cepat tanpa mau ribet setup database server
- Analisis file CSV/Parquet yang gedenya sampai GB-an
- Embedded analytics di dalam aplikasi kamu sendiri
- Ganti pandas buat operasi yang berat di memory, karena DuckDB lebih hemat resource

Kalau kebutuhan kamu OLTP dengan banyak concurrent write (misal backend aplikasi web biasa), DuckDB bukan pilihan yang tepat. Tapi buat kerjaan analitik sehari-hari, ini juara.

#### Penutup
DuckDB ini contoh bagus gimana tool analitik nggak harus ribet buat dipakai. Nggak perlu server, nggak perlu setup macem-macem, tapi performanya tetep bisa diandalkan buat ngolah data gede. Kalau kamu sering kerja sama file CSV/Parquet gede atau butuh analisis cepat, wajib coba.

Dokumentasi lengkapnya bisa dicek di [duckdb.org](https://duckdb.org).
