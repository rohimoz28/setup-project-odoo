# Setup Project Odoo Minimal

Project ini berisi konfigurasi minimal untuk menjalankan Odoo + PostgreSQL dengan Docker. Cocok untuk tutorial, belajar custom module, dan setup yang stabil karena image versinya dipin.

## Isi Project

- `docker-compose.yaml`: menjalankan service `demo-odoo` dan `demo-postgres`.
- `odoo.conf`: konfigurasi minimal Odoo.
- `addons-customize/`: tempat module custom yang kamu buat sendiri.
- `.gitignore`: mengabaikan file lokal seperti `.env`, file IDE, dan cache Python.

## File Yang Perlu Dipakai

Kalau kamu copy-paste project ini ke mesin lain, cukup pastikan file berikut ikut:

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

- `demo-odoo` dan `demo-postgres`: nama service, container, dan host sudah pakai prefix `demo-`.
- image dipin ke versi major dan digest supaya hasil install tetap konsisten.
- `demo-network`: Odoo dan PostgreSQL ada di network yang sama.
- volume:
  - `demo-odoo-data` menyimpan data Odoo.
  - `demo-postgres-data` menyimpan data PostgreSQL.
- port:
  - Odoo bisa dibuka di `http://localhost:8069`
  - PostgreSQL bisa diakses dari database client lewat `localhost:5438`

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

Tidak perlu simpan credential PostgreSQL di `odoo.conf`. Credential database cukup di `docker-compose.yaml`.

## Credential

- PostgreSQL database: `postgres`
- PostgreSQL user: `odoo`
- PostgreSQL password: `odoo`
- Odoo master password: `admindemo`

## Menjalankan Project

```bash
docker compose up -d
```

Setelah itu:

- buka Odoo: `http://localhost:8069`
- akses PostgreSQL dari client: `localhost:5438`

## Command Docker Minimal

- `docker compose up -d`: jalankan semua service di background
- `docker compose down`: stop dan hapus container, network, dan default resources Compose
- `docker compose stop`: stop container tanpa hapus
- `docker compose start`: jalankan container yang sudah ada
- `docker compose restart`: restart semua service
- `docker ps`: lihat container yang sedang jalan

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
