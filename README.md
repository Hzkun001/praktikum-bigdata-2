# Enterprise Batch Data Pipeline (PySpark)

Proyek ini adalah contoh pipeline batch sederhana berbasis PySpark untuk membersihkan, mentransformasi, dan mengagregasi data transaksi e-commerce. Script utama (`scripts/batch_pipeline_enterprise.py`) menjalankan end-to-end alur dari membaca data mentah hingga menyimpan layer clean & curated.

## Ringkasan Fitur
- Validasi & pembersihan data (drop duplikasi, nilai hilang, dan baris bernilai non-positif).
- Konversi tanggal dengan `try_to_date` untuk menjaga data yang valid saja.
- Perhitungan metrik bisnis: total pendapatan per kategori, 5 produk terlaris, dan rata-rata nilai transaksi per pelanggan.
- Penyimpanan output dalam format Parquet termasuk partisi berdasarkan kategori.
- Logging ke `logs/batch_pipeline.log` dan output progres yang jelas di konsol.

## Struktur Direktori
- `scripts/batch_pipeline_enterprise.py` – script pipeline utama.
- `data/raw/ecommerce_raw.csv` – contoh dataset input (CSV dengan header).
- `data/clean/` – hasil clean & transformed dalam Parquet (dibuat saat pipeline dijalankan).
- `data/curated/` – hasil agregasi bisnis dalam Parquet.
- `logs/` – file log eksekusi pipeline.
- `venv/` – (opsional) virtual environment lokal.

## Prasyarat
- Python 3.9+ (direkomendasikan).
- Java Runtime Environment (JRE) yang kompatibel dengan Spark.
- Dependensi Python: `pyspark`.

## Persiapan Lingkungan
```bash
python -m venv venv
source venv/bin/activate   # di Windows: venv\\Scripts\\activate
pip install pyspark
```

## Menjalankan Pipeline
Pastikan file input berada di `data/raw/ecommerce_raw.csv` dengan kolom:
`transaction_id, customer_id, product, category, price, quantity, transaction_date (yyyy-MM-dd)`.

Jalankan:
```bash
python scripts/batch_pipeline_enterprise.py
```
Output yang dihasilkan:
- `data/clean/parquet/` : data transaksi yang sudah dibersihkan + kolom `total_amount`.
- `data/clean/partitioned_by_category/` : versi terpartisi per `category`.
- `data/curated/category_revenue/` : total pendapatan per kategori.
- `data/curated/top_products/` : 5 produk terlaris berdasarkan kuantitas.
- `data/curated/avg_transaction/` : rata-rata nilai transaksi per pelanggan.

## Melihat Hasil Cepat
Gunakan `pyspark` atau `spark-sql` untuk mengecek Parquet, contoh:
```bash
pyspark
spark.read.parquet("data/curated/category_revenue/").show()
```

## Logging & Monitoring
- Log eksekusi: `logs/batch_pipeline.log`.
- Konsol akan menampilkan jumlah record mentah, record bersih, dan ringkasan agregasi.

## Pengembangan Lanjutan
- Tambahkan validasi skema memakai `pydeequ` atau `great_expectations`.
- Tambahkan penulisan ke data lake / warehouse (S3, GCS, atau tabel Hive).
- Jadwalkan dengan Airflow / cron untuk batch periodik.

## Lisensi
Belum ditentukan. Tambahkan lisensi sesuai kebutuhan proyek Anda.
