---
name: ui-end-to-end-flow
description: >-
  Compose end-to-end UI user flows for a single product module in FigCraft or Figma.
  Use when asked to create, reorganize, or scale an end-to-end module flow with happy path,
  branched edge cases, desktop drawers/dialogs, bottom-right toasts, and clean UX copywriting.
---

# UI Module End-to-End Flow Composer

Gunakan skill ini saat menyusun atau merapikan alur layar **end-to-end dalam satu modul** (mis. Web Admin atau Mobile App).

## 1. Struktur Kanvas & Tata Letak Flow

1. **Isolasi per Modul + Section Banner:**
   - Beri 1 `SECTION BANNER` penuh di bagian paling atas modul berisi judul modul dan ringkasan alur (`ALUR UTAMA` & `EDGE CASES`).
   - Setiap layar wajib memiliki **Step Label** tepat di atas frame layarnya (jarak ~`18px`, tinggi `38px`).
2. **Baris 1 = Happy Path (1 Baris Horizontal Kiri -> Kanan):**
   - Susun langkah utama secara berurutan dari langkah konfigurasi/awal hingga hasil akhir dalam **satu baris lurus** dengan pitch seragam (mis. layar `1360px` + gap `80px` = pitch `1440px`).
   - Jangan selipkan layar error/validasi di tengah urutan angka *happy path*.
3. **Baris 2+ = Edge Cases di Bawah Langkah Pemicunya:**
   - **Rule-of-thumb branching:** Edge case yang dipicu pada langkah tertentu wajib diletakkan pada **kolom `x` yang sejajar dengan langkah induknya**, pada baris di bawahnya (`y` baris ke-2).
     - Contoh: error validasi form pada Step 1 ditaruh tepat di bawah Step 1 (`x` sama); error upload file pada Step 4 ditaruh tepat di bawah Step 4 (`x` sama).
   - Edge case yang bersifat global/lintas-halaman (mis. `Empty State` awal, `Timeout 504`, `Role Read-Only`) diberi label `EC-*` terpisah (di kanan setelah happy path atau di baris edge cases tersendiri) tanpa memutus urutan nomor happy path.

## 2. Pola Komponen Standar per Jenis Interaksi

1. **Rincian Data / Sub-Form Aksi (Desktop):** Gunakan **Right Drawer** gaya `shadcn/ui` (**tanpa drag handle pill mobile**).
2. **Konfirmasi Aksi Permanen / Destruktif:** Gunakan **AlertDialog** terpusat gaya `shadcn/ui`.
3. **Hierarki Overlay:** Root frame tetap Auto Layout; gunakan `*Overlay` (`absolute: true`, `opacity: 1`, transparansi hanya di `fills`) yang membungkus `*Content`.
4. **Feedback Pasca-Submit & Error API:** Letakkan **Toast langsung di bawah frame layar utama dengan `absolute: true`** (ignore auto layout, **tanpa wrapper `ToastViewport`**), `constraints: {horizontal: "right", vertical: "bottom"}`, di pojok kanan bawah (`margin 24px` dari kanan & bawah):
   - **Success / Info / Warning:** Tanpa tombol `×`, auto-dismiss 5 detik.
   - **Error:** Wajib ada tombol `×`, tetap tampil sampai ditutup user.

## 3. Checklist Kebersihan UX Writing & Layer Sebelum Selesai

Sebelum menandai flow modul selesai, periksa seluruh teks dan layer di dalam frame layar:
- [ ] Tidak ada kode dokumen/BA (`(BR-xx)`, `(RULE-xx)`, `(SCR-xx)`, `(US-xx)`).
- [ ] Tidak ada catatan rapat/diskusi/FGD (`Catatan FGD...`, `Rekomendasi Rapat...`).
- [ ] Tidak ada label `(contoh)` di judul KPI, tabel, atau komponen UI.
- [ ] Judul & subjudul halaman pada layar `EC-*` identik dengan halaman induknya — tidak menyebut `"Mode Read-Only"`, `"Error Server"`, `"Validasi Gagal"`, atau label skenario test lainnya di header halaman (label skenario hanya di *Step Label* atas frame).
- [ ] Penjelasan/warning yang tersisa murni **UX writing** yang memang membantu pengguna memahami aksi atau error.
- [ ] Seluruh frame pembungkus layout murni (`Workspace Area`, `Main Content Wrapper`, `*Row`, `*Col`, `*Stack`, `*Group`, dll.) sudah **tanpa background fill** (`fills` kosong / dihapus via `object.removeFill`), bukan dibiarkan `#FFFFFF` default atau sekadar `visible: false`.
- [ ] Render screenshot tiap layar baru/berubah dan cek tidak ada teks terpotong atau overlap.
