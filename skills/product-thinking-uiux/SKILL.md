---
name: product-thinking-uiux
description: >-
  Pre-design Product Thinking and Macro User Flow analysis framework for UI/UX
  and Product Designers. Use BEFORE starting new product/feature design,
  redesign, information architecture, wireframing, FigCraft/Figma flow planning,
  or stakeholder handoff boards to analyze business value model, closed-loop
  business flow, core user & business pain points, success metrics, and
  cross-actor macro user flows with zero guesswork.
---

# Product Thinking & Macro User Flow (Pre-Design Gate)

Jalankan kerangka ini **sebelum membuat wireframe, layar UI, atau merombak struktur navigasi** agar setiap keputusan visual, hierarki informasi, dan komponen UI punya alasan bisnis serta perilaku pengguna yang terukur—bukan sekadar estetika atau asumsi.

---

## 1. Prinsip Dasar (Anti-Mistake Rules)

1. **Evidence First, Zero Assumptions:** Selalu baca dokumen sumber proyek yang tersedia lebih dulu (PRD, BA field specs, process map AS-IS/TO-BE, MoM/FGD, kode/desain eksisting) sebelum menyimpulkan model bisnis atau metrik. Jika data kunci belum ada atau bertentangan, tandai eksplisit sebagai `[Perlu Konfirmasi]`—jangan mengarang angka atau aturan bisnis.
2. **Domain-Adaptive Translation (Jangan Pukul Rata Model B2C):** Adaptasikan pertanyaan bisnis sesuai tipe produk:
   - **B2C / Consumer SaaS / Commerce:** Sumber pendapatan (transaksi, langganan, komisi, iklan), konversi akuisisi hingga checkout/retensi.
   - **B2B SaaS / Operational Tool:** Nilai kontrak, efisiensi tim klien, pengurangan error/waktu siklus kerja, adopsi per peran.
   - **B2E / Enterprise Internal Platform:** Tidak memungut uang dari karyawan; ROI berasal dari **kenaikan KPI operasional bisnis** (mis. booking/sales/produktivitas), **efisiensi biaya & jam kerja manual admin**, serta **tata kelola/audit/kepatuhan** (mis. 4-eyes, anonimitas, privasi data).
   - **Marketplace / Multi-Sided Platform:** Insentif suplai & demand, take-rate/monetisasi, friksi pencocokan & trust/safety.
3. **Pisahkan Macro User Flow vs Screen Wireflow:**
   - Jika kanvas desain sudah punya wireframe detail per modul, jangan mengulang daftar setiap klik kecil ke dalam diagram baru.
   - Buat **Macro User Flow (Swimlane Lintas Peran & Sistem)** yang memetakan serah-terima (*handoff*) antar-aktor dan sistem dari pemicu awal hingga hasil bisnis akhir, beserta cabang pengecualian (*exception*) yang berdampak ke risiko bisnis.
4. **Traceability (Product Reason -> UI Consequence):** Setiap pilar Product Thinking wajib diturunkan menjadi implikasi UI nyata (mis. fitur apa yang wajib dominan, guardrail apa yang mengunci aksi, atau metrik apa yang harus terlihat di dashboard).
5. **Hygiene Penempatan Artifak:**
   - Konteks bisnis, kode BA/BRD, dan metrik proyek diletakkan pada **Design Brief / Handoff Page (`Start Here`) / SpecCard**, **TIDAK pernah** diselipkan ke dalam copywriting layar UI end-user kecuali memang bagian dari UI aplikasi yang nyata.

---

## 2. Alur Kerja Eksekusi (5 Gerbang Berurutan)

### Gerbang 1 — Audit Fakta & Konteks Proyek
Kumpulkan dan verifikasi fakta dari artefak proyek:
1. Identifikasi **Tipe Produk** (B2C / B2B / B2E Internal / Marketplace) dan **Batas Fase/Scope** (mis. integrasi API vs upload manual, peran yang masuk vs ditunda).
2. Petakan seluruh **Aktor & Peran** beserta hak akses tulis/setuju/lihat-saja (termasuk *conflict of interest* seperti *4-eyes principle*).
3. Catat **Batasan Teknis & Kepatuhan** (privasi data sensitif, performa perangkat, kebijakan ekonomi/hadiah, audit trail).

### Gerbang 2 — Analisis 4 Pilar Product Thinking
Isi keempat pilar berikut secara ringkas, faktual, dan spesifik terhadap domain produk:

| Pilar | Pertanyaan Inti | Output Wajib Presisi |
| :--- | :--- | :--- |
| **1. Model Nilai Bisnis** | Produk menghasilkan uang / ROI dari mana? Apa pagar ekonominya? | Sumber pendapatan atau ROI operasional (B2E), segmen target prioritas, dan larangan ekonomi/privasi. |
| **2. Flow Bisnis Inti** | Bagaimana perjalanan dari pemicu masuk hingga nilai bisnis selesai terbentuk? | Siklus tertutup (*closed-loop*) lintas tim/sistem: Persiapan Data -> Eksekusi -> Validasi/Settlement -> Evaluasi. |
| **3. Masalah Utama (AS-IS)** | Bagian mana yang paling membuat user & bisnis rugi waktu, uang, atau trust? | Akar friksi pada proses lama (*bottleneck*, proses manual, buta progres, celah kecurangan) + mekanisme solusinya di desain baru. |
| **4. Ukuran Berhasil (KPI)** | Metrik apa yang harus bergerak naik/turun karena desain ini? | 3–5 metrik terukur (baseline AS-IS vs target TO-BE) + perilaku pengguna yang ingin didorong (*North Star* & guardrails). |

### Gerbang 3 — Perancangan Macro User Flow Lintas Aktor
Sebelum masuk ke layout detail per layar:
1. Kelompokkan proses bisnis menjadi **2–4 Jalur Utama (Swimlane)** berdasarkan siklus kerja alamiah produk (mis. *Siklus Data/Transaksi*, *Siklus Tugas Inti & Persetujuan*, *Siklus Tata Kelola/Pengaturan*).
2. Untuk tiap jalur, susun urutan node kiri-ke-kanan berformat seragam:
   - `[PERAN / SISTEM · Referensi Modul/Seksi]`
   - `Nomor + Judul Langkah` (maks. 4–5 kata)
   - `Aksi & Keluaran Konkret` (1 baris)
3. Tambahkan **1 Kotak Cabang Kritis / Kontrol Kunci** di bawah tiap jalur (mis. *failure recovery* jika validasi gagal, pengaman 4-eyes, atau efek pemberlakuan aturan).
4. Cek **3 Uji Kualitas Flow:**
   - **No Dead-End:** Setiap aksi punya lanjutan atau status akhir yang jelas.
   - **Hand-Off Jelas:** Titik pindah tanggung jawab antar peran/sistem (mis. *Admin -> Approver -> Sistem -> Mobile User -> Admin*) terlihat gamblang.
   - **Konsisten dengan Navigasi UI:** Setiap modul pada Macro Flow merujuk ke menu/tab aplikasi yang benar (tidak tercampur antar domain menu).

### Gerbang 4 — Matriks Konsekuensi UI/UX (Design Rationale)
Hubungkan hasil Gerbang 2 & 3 ke keputusan desain nyata dalam tabel 3 kolom:
`Fakta Bisnis / Masalah -> Keputusan Desain UI/UX -> Dampak ke Metrik/Risiko`
Contoh:
- *Program seragam gagal karena sales Lesu & Top butuh dorongan beda* -> **Dashboard menampilkan segmentasi populasi & filter bertingkat; pembuatan misi terpusat per kelompok/parameter** -> *Menaikkan mission completion rate*.
- *Pembuat misi menyetujui misinya sendiri berisiko fraud* -> **Pemisahan peran Pembuat vs Penyetuju + state terkunci pada halaman persetujuan** -> *Menjamin kontrol 4-eyes*.

### Gerbang 5 — Format Penyajian (Markdown vs Kanvas FigCraft/Figma)
1. **Jika user masih meminta analisis / brainstorming:** Sajikan terstruktur dalam Markdown dengan tabel Gerbang 2, ringkasan jalur Gerbang 3, tabel Konsekuensi Gerbang 4, dan daftar `[Perlu Konfirmasi]` jika ada gap.
2. **Jika user meminta ditambahkan ke file desain (FigCraft/Figma):**
   - Tempatkan di halaman pembuka (`00 · Start Here` / `Overview`) di bawah peta navigasi—**bukan** di dalam frame layar produk.
   - **Penamaan Header Board (Anti-Slop):** Pisahkan **Judul** (Nama Produk) dan **Subjudul** (isi konkret board, mis. `Pemetaan Kebutuhan UI/UX, Alur Lintas Peran & Arsitektur Navigasi`). Jangan memakai em dash (`—`), jangan menyalin nama skill/metodologi (`Pre-Design Product Thinking & Macro User Flow`) ke judul utama, dan samakan bahasa dengan isi board.
   - Gunakan Auto Layout vertikal `hug`, **tanpa** `sizingV: "fill"` di dalam container `sizingV: "hug"`, pastikan semua frame wrapper pembungkus transparan (`fills: []`), dan samakan tinggi kartu sejajar (`minHeight`) agar rapi secara optik.
   - **Khusus FigJam (`figma_execute`):** Gunakan `dashPattern` (bukan `strokeDashPattern`) untuk border putus-putus pada `ShapeWithText`, dan beri tinggi minimal `>=145px` pada node `ROUNDED_RECTANGLE` berisi 3+ baris teks agar tidak terpotong elipsis (`...`).
   - Render & inspeksi screenshot seluruh board untuk memverifikasi nol teks terpotong dan nol tabrakan layout.

---

## 3. Checklist Verifikasi Sebelum Selesai (Self-Audit)

- [ ] Seluruh fakta bisnis, peran, dan batasan fase bersumber dari dokumen proyek/konfirmasi user (tidak ada klaim fiktif).
- [ ] Model bisnis diterjemahkan sesuai tipe produk (tidak memaksakan konsep "bayar/iklan" ke produk internal B2E).
- [ ] Macro User Flow menunjukkan lintasan antar-aktor, handoff sistem, dan kontrol/edge branch kritis.
- [ ] Setiap item mengarah pada alasan desain UI/UX atau referensi modul/seksi yang konsisten dengan struktur navigasi aplikasi.
- [ ] Jika diimplementasikan ke FigCraft/Figma: sudah diverifikasi lewat screenshot visual (tidak ada elemen kolaps `0px`, terpotong, atau overlap).
