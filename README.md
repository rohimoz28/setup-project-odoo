# Setup Project Odoo Minimal

Project ini berisi konfigurasi minimal untuk menjalankan Odoo 17 dan PostgreSQL 16 dengan Docker. Setup ini cocok untuk belajar Odoo, membuat custom module, dan menjaga hasil install tetap konsisten karena image Docker sudah dipin dengan digest.

## Isi Project

- `docker-compose.yaml`: menjalankan service `belajar-odoo` dan `belajar-postgres`.
- `odoo.conf`: konfigurasi minimal Odoo.
- `addons-customize/`: folder untuk custom module Odoo yang akan dimount ke container.

## File Yang Perlu Dipakai

Kalau kamu copy-paste project ini ke mesin lain, pastikan file dan folder berikut ikut:

- `docker-compose.yaml`
- `odoo.conf`
- `addons-customize/`

Folder `addons-customize/` boleh kosong dulu. Nanti isi dengan module Odoo buatanmu.

## Cara Install

### Opsi 1. Clone repository

```bash
git clone <repository-url>
cd setup-project-odoo
```

### Opsi 2. Copy-paste config

Kalau tidak pakai `git clone`, buat file dan folder ini di root project:

```text
docker-compose.yaml
odoo.conf
addons-customize/
```

## Konfigurasi Minimal

### `docker-compose.yaml`

Bagian pentingnya:

- `belajar-odoo`: service Odoo 17.
- `belajar-postgres`: service PostgreSQL 16.
- `belajar-network`: network bridge agar Odoo dan PostgreSQL bisa saling terhubung.
- `belajar-odoo-data`: volume untuk menyimpan data Odoo.
- `belajar-postgres-data`: volume untuk menyimpan data PostgreSQL.
- `./odoo.conf:/etc/odoo/odoo.conf:ro`: mount config Odoo dalam mode read-only.
- `./addons-customize:/mnt/extra-addons`: mount folder custom addons ke container Odoo.
- `8069:8069`: Odoo bisa dibuka lewat `http://localhost:8069`.
- `5438:5432`: PostgreSQL bisa diakses dari host lewat `localhost:5438`.

Image Docker yang dipakai:

- `odoo:17.0`
- `postgres:16`

Keduanya dipin dengan digest di `docker-compose.yaml` supaya hasil install lebih konsisten.

### `odoo.conf`

Isi minimalnya seperti ini:

```ini
[options]
admin_passwd = admindemo
addons_path = /usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons
data_dir = /var/lib/odoo
```

Fungsi tiap baris:

- `admin_passwd`: master password untuk Database Manager Odoo.
- `addons_path`: lokasi core addons dan custom addons.
- `data_dir`: lokasi data Odoo di dalam container.

Credential PostgreSQL tidak disimpan di `odoo.conf`. Credential database diatur lewat `docker-compose.yaml`.

## Credential

- PostgreSQL database: `postgres`
- PostgreSQL user: `odoo`
- PostgreSQL password: `odoo`
- Odoo master password: `admindemo`

## Menjalankan Project

```bash
docker compose up -d
```

Setelah container berjalan:

- buka Odoo: `http://localhost:8069`
- akses PostgreSQL dari database client: `localhost:5438`

## Command Docker Minimal

- `docker compose up -d`: jalankan semua service di background.
- `docker compose down`: stop dan hapus container, network, dan default resources Compose.
- `docker compose stop`: stop container tanpa hapus.
- `docker compose start`: jalankan container yang sudah ada.
- `docker compose restart`: restart semua service.
- `docker ps`: lihat container yang sedang jalan.

## Logs

Lihat semua logs:

```bash
docker compose logs -f --tail 200
```

Lihat logs service tertentu:

```bash
docker compose logs -f --tail 200 belajar-odoo
docker compose logs -f --tail 200 belajar-postgres
```

## Cara Pakai `addons-customize`

Taruh module custom kamu di folder `addons-customize/`. Setelah itu restart Odoo supaya module terbaca lagi.

Contoh alur singkat:

```bash
docker compose restart belajar-odoo
```

Kalau module baru belum muncul di Apps, masuk ke Odoo lalu update Apps List.

## Catatan Singkat

- Project ini sengaja minimal.
- Tidak ada config yang tidak perlu.
- Tujuannya supaya tutorial mudah diikuti dan hasilnya tetap sama di mesin lain.
