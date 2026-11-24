# Caliper UI/UX Design Standards (Figma & FigCraft)

## 1. Struktur Frame Overlay & Popup UI (Modal, Dialog, Drawer, Sheet)

Setiap kali membuat UI yang membutuhkan *background overlay* atau komponen popup (**Modal, Dialog, AlertDialog, Drawer, Sheet, Popover penuh**), ikuti struktur hierarki berikut:

1. **Pertahankan Auto Layout pada Frame Layar Utama (Root Screen):**
   - Dilarang mengubah `layout` frame layar utama menjadi `null` / None hanya untuk menaruh overlay.
   - Halaman utama di belakang overlay tetap berada di alur auto-layout normal.
2. **Gunakan Satu Frame `*Overlay` dengan `absolute: true`:**
   - Buat frame overlay (`AlertDialogOverlay`, `DialogOverlay`, `DrawerOverlay`, `SheetOverlay`) di level atas layar (`x: 0, y: 0`, ukuran penuh layar, `cornerRadius` mengikuti layar, `clip: true`).
   - Aktifkan **`absolute: true`** (ignore auto layout) dengan `constraints: {horizontal: "left", vertical: "top"}` agar mengambang penuh di atas halaman tanpa merusak auto-layout layar utama.
3. **Transparansi Scrim Wajib pada Paint (`fills`), Bukan Properti `opacity` Layer:**
   - Set properti layer **`opacity: 1`** pada frame `*Overlay`.
   - Atur transparansi scrim **hanya pada paint fill**, contoh: `fills: [{"type": "solid", "color": "#09090B", "opacity": 0.25}]`. Ini menjaga konten popup di dalamnya tetap solid 100%.
4. **Komponen Popup (`*Content`) Wajib Menjadi Child Langsung di Dalam Frame `*Overlay`:**
   - Dilarang menaruh komponen popup sejajar (*sibling*) dengan overlay.
   - Atur posisi komponen melalui **Auto Layout milik frame `*Overlay`**:
     - **Modal / Dialog / AlertDialog (Terpusat):** `layout: "horizontal"` (atau `"vertical"`), `mainAlign: "center"`, `crossAlign: "center"`, `padding: 24`. Child `*Content` (`width` tetap, `sizingV: "hug"`).
     - **Right Drawer / Sheet (Desktop):** `layout: "horizontal"`, `mainAlign: "max"`, `crossAlign: "center"`, `padding: 12` (atau `[0,0,0,0]` jika flush). Child `*Content` (`width` tetap mis. `472`, `sizingV: "fill"`, tanpa drag handle mobile pada desktop view).
     - **Left Drawer / Sheet:** `layout: "horizontal"`, `mainAlign: "min"`, `crossAlign: "center"`, `padding: 12`.
     - **Bottom Drawer / Sheet (Mobile):** `layout: "vertical"`, `mainAlign: "max"`, `crossAlign: "center"`.

## 2. Copywriting UI Mockup (Bebas Catatan Dokumen & Meeting)

1. **Dilarang Memasukkan Catatan Dokumen atau Hasil Meeting ke Layar UI:**
   - Jangan tampilkan kode dokumen/requirement internal (`BR-xx`, `RULE-xx`, `SCR-Wxx`, `US-xx`), catatan rapat/FGD (`Catatan FGD...`, `Hasil Rapat...`), maupun penanda contoh (`(contoh)`) di judul halaman, kartu, tombol, label tabel, atau footer layar aplikasi.
   - Gunakan angka & data dummy yang realistis tanpa label `(contoh)`.
2. **Hanya Tampilkan Teks End-User:**
   - Teks penjelasan di layar hanya boleh ada jika memang dibutuhkan pengguna akhir (peringatan sebelum submit permanen, label bantuan input, empty state, pesan error), ditulis dalam bahasa produk yang natural.
3. **Judul Halaman Konsisten Tanpa Label Mode / Skenario:**
   - Dilarang menempelkan nama state, role, atau skenario test pada judul halaman di dalam layar aplikasi (mis. `Hadiah & Pencairan — Mode Read-Only`).
   - Letakkan label skenario (`EC-1`, `Mode Read-Only`, `Error 504`) hanya di **Step Label di luar/atas frame layar**.

## 3. Penamaan Judul Board & Dokumen UI/UX (Anti-Slop)

1. **Pisahkan Judul & Subjudul, Hindari Em Dash (`—`):**
   - Gunakan hierarki **Judul** (nama produk/proyek) + **Subjudul** (tujuan/isi konkret board), atau titik dua (`:`) untuk label singkat.
2. **Jangan Bocorkan Nama Skill atau Jargon Metodologi ke Judul Stakeholder:**
   - Hindari istilah meta-proses (*Pre-Design*, *Product Thinking*, *Macro User Flow*) di judul utama stakeholder. Tuliskan isi nyatanya dalam bahasa yang lugas.
3. **Konsistensi Bahasa & Ringkas:**
   - Samakan bahasa subjudul dan label section dengan bahasa utama konten board.

## 4. Notifikasi Toast Pasca-Submit

1. **Tanpa Wrapper Viewport:** Jangan membungkus Toast dengan frame `ToastViewport` seukuran layar penuh. Letakkan Toast langsung di bawah frame layar utama dengan **`absolute: true`**, `constraints: {horizontal: "right", vertical: "bottom"}`, posisi pojok kanan bawah (`margin 24px`).
2. **Success / Info / Warning Toast:** Tanpa tombol tutup (`×`), hilang otomatis setelah **5 detik**.
3. **Error Toast:** Wajib memiliki tombol tutup (`×`) dan tetap tampil sampai pengguna menutupnya.

## 5. Transparansi Frame Layout Wrapper

Setiap frame yang berfungsi murni sebagai pembungkus struktur/Auto Layout (`Main Content Wrapper`, `*Row`, `*Col`, `*Stack`, `*Group`, `DrawerHeader`, `DrawerBody`, `Filter Group`, dsb.) **wajib transparan tanpa background fill (`fills: []`)**:
1. Segera hapus fill default `#FFFFFF` pada semua frame non-kartu, non-pill, dan non-overlay.
2. Background fill hanya boleh ada pada root screen, kartu konten, tabel header, pill/badge/tombol, drawer/dialog `*Content`, toast, dan `*Overlay` scrim.
3. Jangan sekadar menyembunyikan fill (`visible: false`); hapus fill-nya agar bersih untuk token binding dan Light/Dark Mode.
