# Setup Guide: AR Menu di GitHub Pages + Custom Domain

## STEP 1: Buat Repository di GitHub

1. Login ke GitHub: https://github.com
2. Klik tombol **+** (kanan atas) → **New repository**
3. Isi:
   - Repository name: `alur-cerdas`
   - Description: `AR Menu Platform`
   - Pilih **Public** (wajib untuk GitHub Pages gratis)
   - ✅ Centang "Add a README file"
4. Klik **Create repository**

## STEP 2: Upload File ke Repository

### Cara A: Lewat GitHub Web (Paling Mudah)

1. Buka repo `alur-cerdas` yang baru dibuat
2. Klik **Add file** → **Upload files**
3. Drag & drop SELURUH folder `restoran-a/` ke area upload
4. Tulis commit message: "Add restoran-a AR menu"
5. Klik **Commit changes**

**PENTING:** Pastikan struktur di repo terlihat seperti ini:
```
alur-cerdas/
├── README.md
├── restoran-a/
│   ├── index.html
│   ├── images/
│   │   ├── burger.jpg
│   │   └── pocari.jpg
│   └── models/
│       ├── burger/
│       │   ├── scene.glb    ← REPLACE dengan file asli
│       │   └── scene.usdz   ← REPLACE dengan file asli
│       └── pocari/
│           ├── scene.glb    ← REPLACE dengan file asli
│           └── scene.usdz   ← REPLACE dengan file asli
```

### Cara B: Lewat Git CLI

```bash
git clone https://github.com/USERNAME/alur-cerdas.git
cd alur-cerdas
# Copy folder restoran-a ke sini
cp -r /path/to/restoran-a .
git add .
git commit -m "Add restoran-a AR menu"
git push origin main
```

## STEP 3: Replace Placeholder Model Files

File `.glb` dan `.usdz` yang ada sekarang adalah PLACEHOLDER.
Kamu harus replace dengan file 3D asli:

1. Di repo GitHub, navigate ke `restoran-a/models/burger/`
2. Klik `scene.glb` → klik ikon **Delete** (trash icon) → Commit
3. Klik **Add file** → **Upload files**
4. Upload file `.glb` asli kamu (rename ke `scene.glb`)
5. Ulangi untuk `scene.usdz`, dan untuk folder `pocari/`

**Atau lewat Git CLI:**
```bash
cd alur-cerdas/restoran-a/models/burger/
# Hapus placeholder dan copy file asli
rm scene.glb scene.usdz
cp /path/to/burger-model.glb scene.glb
cp /path/to/burger-model.usdz scene.usdz
# Sama untuk pocari
cd ../pocari/
rm scene.glb scene.usdz
cp /path/to/pocari-model.glb scene.glb
cp /path/to/pocari-model.usdz scene.usdz
# Push
cd ../../../
git add .
git commit -m "Add real 3D model files"
git push origin main
```

## STEP 4: Enable GitHub Pages

1. Buka repo → **Settings** (tab paling kanan)
2. Scroll ke sidebar kiri → klik **Pages**
3. Di bagian **Source**:
   - Branch: `main`
   - Folder: `/ (root)`
4. Klik **Save**
5. Tunggu 1-2 menit, GitHub akan deploy

**Test dulu tanpa custom domain:**
Buka: `https://USERNAME.github.io/alur-cerdas/restoran-a/`
Menu harus muncul.

## STEP 5: Setup Custom Domain

### 5a. Di GitHub:
1. Masih di **Settings** → **Pages**
2. Di bagian **Custom domain**, isi: `alur-cerdas.com`
3. Klik **Save**
4. ✅ Centang **Enforce HTTPS** (setelah DNS propagate)

### 5b. Di Domain Provider (tempat beli domain):
Masuk ke DNS management domain `alur-cerdas.com` dan tambahkan records berikut:

**Opsi 1: Apex domain (alur-cerdas.com langsung)**

| Type  | Name | Value                    |
|-------|------|--------------------------|
| A     | @    | 185.199.108.153          |
| A     | @    | 185.199.109.153          |
| A     | @    | 185.199.110.153          |
| A     | @    | 185.199.111.153          |
| CNAME | www  | USERNAME.github.io       |

**Opsi 2: Subdomain (misalnya ar.alur-cerdas.com)**

| Type  | Name | Value              |
|-------|------|--------------------|
| CNAME | ar   | USERNAME.github.io |

Ganti `USERNAME` dengan GitHub username kamu.

### 5c. Buat file CNAME di repo:
1. Di root repo, klik **Add file** → **Create new file**
2. Nama file: `CNAME`
3. Isi: `alur-cerdas.com` (atau `ar.alur-cerdas.com` jika pakai subdomain)
4. Commit

### 5d. Tunggu DNS Propagation
- Biasanya 5-30 menit, bisa sampai 24 jam
- Cek di: https://dnschecker.org

## STEP 6: Test di HP

Setelah live, buka di HP:
1. `alur-cerdas.com/restoran-a`  (atau `ar.alur-cerdas.com/restoran-a`)
2. Menu muncul
3. Tap gambar burger → AR terbuka dengan model 3D
4. Tap gambar Pocari → AR terbuka dengan model 3D

### Troubleshooting:
- **AR tidak muncul di Android?** → Pastikan Google app terinstall, dan file .glb bukan placeholder
- **AR tidak muncul di iOS?** → Pastikan file .usdz valid dan bukan placeholder
- **404 error?** → Cek struktur folder di repo, pastikan ada `restoran-a/index.html`
- **HTTPS error?** → Tunggu DNS propagation selesai, lalu enable "Enforce HTTPS" di GitHub Pages settings
- **Model terlalu besar/kecil di AR?** → Adjust scale di 3D software sebelum export

## Menambah Restoran Baru

Untuk client baru, tinggal:
1. Duplicate folder `restoran-a` → rename jadi `restoran-b` (atau nama client)
2. Edit `index.html` di dalamnya (ganti menu, gambar, harga)
3. Upload model .glb/.usdz baru
4. Push ke repo
5. Akses: `alur-cerdas.com/restoran-b`

Tidak perlu setup DNS atau GitHub Pages ulang — semua otomatis!
