# Setup Project Odoo Minimal

Project ini berisi konfigurasi minimal untuk menjalankan Odoo 19 dan PostgreSQL 16 dengan Docker. Setup ini cocok untuk belajar Odoo, membuat custom module, dan menjaga hasil install tetap konsisten.

## Isi Project

- `docker-compose.yaml`: menjalankan service `demo-odoo` dan `demo-postgres`.
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

- `demo-odoo`: service Odoo 19.
- `demo-postgres`: service PostgreSQL 16.
- `demo-network`: network bridge agar Odoo dan PostgreSQL bisa saling terhubung.
- `demo-odoo-data`: volume untuk menyimpan data Odoo.
- `demo-postgres-data`: volume untuk menyimpan data PostgreSQL.
- `./odoo.conf:/etc/odoo/odoo.conf:ro`: mount config Odoo dalam mode read-only.
- `./addons-customize:/mnt/extra-addons`: mount folder custom addons ke container Odoo.
- `80:8069`: Odoo bisa dibuka lewat `http://localhost` atau langsung dari IP address host.
- `5432:5432`: PostgreSQL bisa diakses dari host lewat `localhost:5432`.

Image Docker yang dipakai:

- `odoo:19.0`
- `postgres:16-bookworm`

Keduanya menggunakan image Debian-based.

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

- buka Odoo: `http://localhost`
- akses PostgreSQL dari database client: `localhost:5432`

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
docker compose logs -f --tail 200 demo-odoo
docker compose logs -f --tail 200 demo-postgres
```

## Cara Pakai `addons-customize`

Taruh module custom kamu di folder `addons-customize/`. Setelah itu restart Odoo supaya module terbaca lagi.

Contoh alur singkat:

```bash
docker compose restart demo-odoo
```

Kalau module baru belum muncul di Apps, masuk ke Odoo lalu update Apps List.

## Catatan Singkat

- Project ini sengaja minimal.
- Tidak ada config yang tidak perlu.
- Tujuannya supaya tutorial mudah diikuti dan hasilnya tetap sama di mesin lain.
