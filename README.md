# 🍽️ AR Menu Viewer — FnB
Sistem AR Menu 3D siap pakai untuk industri F&B. Tinggal tambahkan assets dan deploy!

---

## 📁 Struktur Folder

```
ar-menu/
├── index.html          ← Aplikasi utama (tidak perlu diubah)
├── menu.json           ← Data menu restoran (EDIT INI untuk tiap klien)
└── assets/
    ├── models/         ← File 3D model (.glb / .gltf)
    │   ├── nasi-goreng.glb
    │   ├── sate-ayam.glb
    │   └── ...
    └── markers/        ← File marker AR (.patt)
        ├── hiro.patt   ← Marker default (bisa download dari AR.js)
        └── custom.patt ← Marker custom per item (opsional)
```

---

## 🚀 Cara Deploy

### 1. Edit `menu.json`
Ubah bagian `config` untuk nama restoran:
```json
{
  "config": {
    "name": "Nama Restoran Klien",
    "tagline": "✦ Tagline Restoran",
    "desc": "Deskripsi singkat untuk landing page.",
    "accent": "#C9A84C"
  },
  "items": [...]
}
```

### 2. Tambahkan Assets
- **Model 3D** → taruh di `assets/models/` (format `.glb` disarankan)
  - Tools gratis: Blender, Spline, Sketchfab (download .glb)
  - Bisa juga pakai URL eksternal (CDN)
- **Marker AR** → taruh di `assets/markers/`
  - Download `hiro.patt` dari: https://ar-js-org.github.io/AR.js/
  - Custom marker: https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html

### 3. Upload ke Hosting
Upload seluruh folder ke hosting apapun yang mendukung static files:
- Netlify (gratis, drag & drop)
- Vercel (gratis)
- cPanel / File Manager hosting biasa
- GitHub Pages (gratis)

### 4. HTTPS Wajib!
AR memerlukan akses kamera → wajib pakai HTTPS. Netlify & Vercel sudah otomatis HTTPS.

---

## ⚙️ Admin Panel (Built-in)

Klik ikon ⚙ di pojok kanan bawah untuk:
- Tambah/edit/hapus item menu
- Ubah konfigurasi restoran (nama, warna, tagline)
- Export `menu.json` yang sudah diperbarui

Data tersimpan di **LocalStorage** browser sebagai fallback jika `menu.json` tidak ada.

---

## 📐 Cara Kerja AR

| Mode | Cara Kerja |
|------|-----------|
| **Preview 3D** | Model 3D bisa diputar/zoom langsung di layar (Google Model Viewer) |
| **Mode AR (Marker)** | Buka kamera → arahkan ke marker fisik → model 3D muncul di atas meja |

### Marker yang Didukung
- **Hiro** (default, paling stabil) — marker bawaan AR.js
- **Custom pattern** — bisa upload gambar logo restoran sebagai marker

---

## 🔧 Konfigurasi Per-Item di menu.json

```json
{
  "id": "item_unik",
  "name": "Nama Menu",
  "category": "Makanan Utama",
  "desc": "Deskripsi singkat.",
  "price": "Rp 45.000",
  "img": "assets/foto/nama-menu.jpg",    ← Foto untuk card menu
  "model": "assets/models/nama.glb",     ← Model 3D (kosongkan jika belum ada)
  "marker": "assets/markers/hiro.patt"   ← File marker AR
}
```

---

## ⚠️ Kendala Multi-Klien di 1 Hosting

Baca bagian ini sebelum deploy untuk beberapa klien!

### ✅ Yang Bisa Dilakukan
1. **Subfolder per klien** — `domain.com/klien-a/`, `domain.com/klien-b/`
   - Masing-masing punya `menu.json` sendiri
   - Paling simpel, tidak perlu backend

2. **Subdomain per klien** — `klien-a.domain.com`, `klien-b.domain.com`
   - Lebih profesional, perlu konfigurasi DNS

### ❌ Kendala Utama (Fleksibilitas)

**1. Tidak ada isolasi data**
LocalStorage tersimpan per domain/origin. Jika semua klien di subdomain berbeda, fine. Tapi jika di subfolder yang sama, localStorage bisa tercampur.

**2. Satu warna aksen per deployment**
Setiap klien punya branding berbeda. Dengan static file, tiap klien butuh folder/deployment sendiri.

**3. Assets terpisah wajib**
Model 3D tiap klien harus di folder berbeda. Tidak bisa share assets antar klien (kecuali via CDN eksternal).

**4. Tidak ada dashboard terpusat**
Tidak ada satu panel untuk mengelola semua klien sekaligus. Harus buka admin panel tiap klien satu per satu.

**5. Update massal susah**
Jika ada bug fix di `index.html`, harus update file di semua folder klien secara manual.

### 💡 Solusi Rekomendasi untuk Multi-Klien Skala Besar
→ Gunakan backend sederhana (Node.js/PHP) dengan database
→ Setiap klien punya `slug` unik: `domain.com/menu/warung-A`
→ `menu.json` diganti dengan API: `/api/menu?client=warung-A`
→ Satu codebase, banyak klien, dashboard terpusat

---

## 📱 Kompatibilitas

| Browser | Preview 3D | Mode AR |
|---------|-----------|---------|
| Chrome Android | ✅ | ✅ |
| Safari iOS | ✅ | ✅ (via WebXR) |
| Chrome Desktop | ✅ | ✅ (butuh webcam) |
| Firefox | ✅ | ⚠️ Terbatas |

---

## 📞 Teknologi yang Digunakan

- **A-Frame** — 3D/VR framework berbasis WebGL
- **AR.js** — Marker-based AR untuk web
- **Google Model Viewer** — Preview 3D interaktif
- **Vanilla JS + CSS** — Tidak perlu build tools, langsung jalan

---

*Made for FnB Industry — Tinggal deploy, tinggal scan, langsung WOW!* 🚀
