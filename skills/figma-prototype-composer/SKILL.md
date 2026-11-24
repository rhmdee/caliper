---
name: figma-prototype-composer
description: >-
  Use when building, wiring, or refactoring interactive, variable-driven prototypes in Figma without screen or variant duplication. Enforces comprehensive instance property scanning for all bindable properties (variant, text, boolean, instance swap, state, invalid/validation), atomic multi-action reaction chaining (SET_VARIABLE, OVERLAY, CLOSE), dynamic filters and tabs, interactive forms, bottom sheet pickers, and single-screen data reactivity.
---

# Figma Prototype Composer

This skill governs the end-to-end process of authoring production-grade, variable-driven interactive prototypes in Figma. It eliminates screen and variant duplication by driving UI state directly through Figma Variables and atomic multi-action reaction chaining.

---

## 1. Operating Principles

1. **Mandatory Exhaustive Instance Property Scanning (Scan-First Rule):**
   * Before binding variables or wiring interactions, **NEVER assume or bind only one superficial property**.
   * ALWAYS perform an exhaustive scan across the entire instance and all its nested sub-instances to discover every bindable property:
     - **Variant Properties:** `variant` (e.g. `primary` vs `outline`), `state` (`idle`, `focused`, `filled`, `hover`, `disabled`), `size`, `type`.
     - **Input Field State Synchronization (`state: "filled"` vs `"idle"`):** Saat mengikat nilai teks ke input field, DILARANG membiarkan nested `._field-base` berada di state `"idle"`. State `"idle"` menyebabkan teks tampak seperti placeholder redup/abu-abu meskipun nilainya sudah berubah. Wajib mengikat atau mengubah properti `state` pada `._field-base` menjadi `"filled"` agar teks dirender tegas dengan kontras tinggi (*active filled input*).
     - **Text Properties:** `text#...` (input values), `label#...`, `helper-text#...`, `invalid-text#...`, `description#...`.
     - **Boolean Properties:** `invalid#...` (error trigger), `has-helper#...`, `has-label#...`, `mandatory#...`, `showLeadingIcon`, `showTrailingIcon`.
     - **Instance Swap Properties:** `icon#...` (icon swaps).
     - **Layer Properties:** `visible` (conditional rendering), `characters` (text nodes).
2. **Zero-Screen Duplication (Pure Data-Driven Prototyping):**
   * Dilarang menduplikasi artboard layar penuh hanya untuk merepresentasikan perbedaan status (misal: layar tenor 12 vs 24 vs 36 bulan, atau tab Tindakan vs Proses vs Selesai).
   * Seluruh perubahan visual dan data dikendalikan melalui mutasi Figma Variables di dalam satu frame layar tunggal.
3. **Zero-Variant Sprawl (Variable-Bound Variant Aliasing):**
   * Hindari membuat Component Set raksasa berisi belasan kombinasi varian dengan transisi `Change to`.
   * Cukup satu komponen master dengan child instance yang properti `variant`-nya di-bind ke String Variable (`VARIABLE_ALIAS`).
4. **Atomic Multi-Action Reaction Chaining:**
   * Setiap interaksi `ON_CLICK` mengeksekusi array `actions` lengkap secara serentak:
     1. Mutasi status visual tombol/tab aktif (misal: `"primary"` vs `"outline"`).
     2. Mutasi seluruh variabel kalkulasi atau data numerik turunan.
     3. Sinkronisasi tampilan teks di layar ringkasan atau detail lintas-layar.
5. **Realistic Input Simulation (Pickers & Preset Chips):**
   * Karena Figma runtime belum mendukung pengetikan keyboard fisik langsung ke variabel, simulasikan input menggunakan:
     - **Bottom Sheet / Dropdown Picker Overlay:** Untuk input katalog (misal tipe kendaraan, provinsi/cabang).
     - **Quick Preset Chips:** Untuk nominal angka (misal `[80 Jt]`, `[100 Jt]`, `[120 Jt]`, `[140 Jt]`) yang dilengkapi pemicu validasi error `invalid = true` saat over-limit.
6. **Canvas Cleanliness & Section Dignity:**
   * Seluruh layar alur mobile wajib berjajar rapi dalam satu baris horizontal dengan jarak konsisten (misal: `32px` antar-layar).
   * Dilarang meletakkan master komponen atau bottom sheet terapung sembarangan di margin kanvas. Letakkan master komponen di area terisolasi berlabel dan bottom sheet sejajar dalam flow.
7. **The Dual-Property Input Field Contract (`text` + `state: "filled"` Synchronization):**
   * Pada komponen input field (seperti `field` dengan sub-instance `._field-base`), mengubah atau mengikat properti nilai `text#...` SAJA adalah sebuah kecacatan visual (*anti-pattern*).
   * State `idle` membawa styling placeholder (warna teks redup/abu-abu).
   * **Aturan Wajib:** Setiap kali sebuah kotak input memiliki nilai terisi atau nilainya dimutasi oleh interaksi/variabel, properti `state` pada `._field-base` WAJIB diikat ke variabel atau diset ke `"filled"` agar teks dirender tegas dengan kontras tinggi (*high-contrast active input styling*).
8. **Anti-100px Frame Geometry Guard (Strict Programmatic Hugging):**
   * Di Figma Plugin API, setiap panggilan `figma.createFrame()` secara otomatis menginisialisasi ukuran bawaan `100×100 px` dengan `layoutSizingVertical = "FIXED"`.
   * **Aturan Wajib:** Setiap pembuatan Auto Layout frame baru (baris chip, label row, modal header, wrapper) WAJIB secara eksplisit mengeset:
     - `node.layoutSizingVertical = "HUG"` (kecuali viewport layar penuh fixed 812px)
     - `node.layoutSizingHorizontal = "FILL"` (atau `"HUG"` sesuai kebutuhan)
   * Selain itu, seluruh child button pada baris berlebar tetap wajib mengeset sub-instance `._button-base.layoutSizingHorizontal = "FILL"` untuk mencegah teks meluap keluar (*overflow*).

---

## 2. Phase-by-Phase Execution Workflow

### Phase 1: Exhaustive Instance Property Discovery (Scanning)
Sebelum menyuntikkan binding atau interaksi, jalankan script inspeksi untuk memetakan seluruh properti instance yang dapat di-bind:

```javascript
// Universal Instance Property Scanner
async function scanInstanceProperties(node) {
  const report = {
    id: node.id,
    name: node.name,
    bindableComponentProperties: {},
    subInstances: [],
    boundVariables: node.boundVariables || {}
  };

  // 1. Scan Component Properties pada root instance
  if (node.componentProperties) {
    for (const [key, prop] of Object.entries(node.componentProperties)) {
      report.bindableComponentProperties[key] = {
        type: prop.type, // 'VARIANT' | 'TEXT' | 'BOOLEAN' | 'INSTANCE_SWAP'
        value: prop.value,
        isBound: !!(prop.boundVariables && prop.boundVariables.value)
      };
    }
  }

  // 2. Scan Sub-instances (misal: ._field-base, ._button-base, badge)
  if (node.findAll) {
    const subInsts = node.findAll(n => n.type === 'INSTANCE' && n.id !== node.id);
    for (const sub of subInsts) {
      const subProps = {};
      if (sub.componentProperties) {
        for (const [k, p] of Object.entries(sub.componentProperties)) {
          subProps[k] = { type: p.type, value: p.value };
        }
      }
      report.subInstances.push({
        id: sub.id,
        name: sub.name,
        props: subProps,
        boundVariables: sub.boundVariables || {}
      });
    }
  }

  return report;
}
```

---

### Phase 2: Variable Architecture & Creation
Kelompokkan variabel di dalam koleksi khusus prototype (misal: `Prototype_Simulation`):
1. **Control State Variables (STRING):** Variabel penyimpan varian visual (misal: `select_tenor/tenor_12 = "primary"`, `tab_badge_tindakan = "primary"`).
2. **Numeric & Currency Variables (STRING / FLOAT):** Penyimpan nominal cicilan, biaya admin, asuransi, plafon (misal: `sim_nominal_plafon = "Rp 120.000.000"`).
3. **Validation & Visibility Variables (BOOLEAN):**
   - `sim_is_invalid = false` (Memicu border merah dan pesan error pada input field).
   - `show_card_tindakan = true`, `show_card_proses = false` (Menampilkan/menyembunyikan kartu di tab list).

---

### Phase 3: Exhaustive Property Binding Contract

Gunakan skema API resmi Figma untuk mengikat properti ke variabel:

#### 1. Binding Component Properties (Variant, Text, Boolean)
```javascript
// Binding properti component ke Variable Alias
targetInstance.setProperties({
  // Variant binding
  "variant": { type: "VARIABLE_ALIAS", id: stringVarId },
  
  // Text input binding
  "text#85:0": { type: "VARIABLE_ALIAS", id: textVarId },
  "helper-text#85:34": { type: "VARIABLE_ALIAS", id: helperVarId },
  
  // Validation state binding
  "invalid#85:44": { type: "VARIABLE_ALIAS", id: boolVarId },
  "invalid-text#85:39": { type: "VARIABLE_ALIAS", id: errorTextVarId }
});
```

#### 2. Binding Layer Visibility (Conditional Rendering)
```javascript
// Mengikat visibilitas frame/kartu ke variabel Boolean
cardNode.setBoundVariable("visible", booleanVarObj);
```

#### 3. Binding Text Node Characters
```javascript
// Mengikat teks statis ke variabel String
textNode.setBoundVariable("characters", stringVarObj);
```

#### 4. The Dual-Binding Contract for Form Inputs (`field` + `._field-base`)
Saat mengikat kotak input yang nilainya terisi secara default atau dapat dipilih via picker/chip:
```javascript
// 1. Bind nilai teks
fieldBase.setProperties({
  "text#85:0": { type: "VARIABLE_ALIAS", id: textVarId }
});

// 2. WAJIB bind atau ubah state menjadi "filled" (mencegah teks tampak seperti placeholder redup)
fieldBase.setProperties({
  "state": { type: "VARIABLE_ALIAS", id: stateVarId } // atau "state": "filled"
});
```

---

### Phase 4: Atomic Multi-Action Reaction Chaining

Figma Plugin API mewajibkan format `actions: [ ... ]` pada trigger prototype.

```javascript
// Skema Rantai Aksi Multi-Action Valid
function makeSetVar(variableId, type, value) {
  return {
    type: "SET_VARIABLE",
    variableId,
    variableValue: {
      type,
      resolvedType: type,
      value
    }
  };
}

// Pasang reaksi atomik
await buttonNode.setReactionsAsync([{
  trigger: { type: "ON_CLICK" },
  actions: [
    // 1. Toggle visual varian tombol
    makeSetVar(varSelfId, "STRING", "primary"),
    makeSetVar(varOtherId, "STRING", "outline"),
    
    // 2. Update data kalkulasi
    makeSetVar(varPlafonId, "STRING", "Rp 120.000.000"),
    makeSetVar(varCicilanId, "STRING", "Rp 4.120.000 / bln"),
    makeSetVar(varInvalidId, "BOOLEAN", false),
    
    // 3. Sinkronisasi tampilan di layar lain
    makeSetVar(varDetailDisplayId, "STRING", "Plafon: Rp 120.000.000")
  ]
}]);
```

---

### Phase 5: Realistic Input Simulation Mechanics

#### A. Bottom Sheet Catalog Picker (Overlay Navigation)
1. **Trigger pada Field:**
   ```javascript
   await fieldInstance.setReactionsAsync([{
     trigger: { type: "ON_CLICK" },
     actions: [{
       type: "NODE",
       destinationId: bottomSheetFrameId,
       navigation: "OVERLAY",
       transition: {
         type: "DISSOLVE", // atau SMART_ANIMATE
         easing: { type: "EASE_OUT" },
         duration: 0.2
       }
     }]
   }]);
   ```
2. **Item Selection di dalam Bottom Sheet:**
   Menggabungkan `SET_VARIABLE` dengan aksi `CLOSE` penutup overlay:
   ```javascript
   await itemCard.setReactionsAsync([{
     trigger: { type: "ON_CLICK" },
     actions: [
       makeSetVar(varTipeId, "STRING", "Toyota Fortuner 2.4 VRZ A/T 2021"),
       makeSetVar(varLtvHelperId, "STRING", "Maks. LTV 80% (Rp 328.000.000) • Plafon Fleksibel"),
       makeSetVar(varNominalId, "STRING", "Rp 250.000.000"),
       makeSetVar(varCicilanId, "STRING", "Rp 8.580.000 / bln"),
       { type: "CLOSE" } // Otomatis menutup Bottom Sheet
     ]
   }]);
   ```

#### B. Quick Preset Chips (Direct Field & Validation Simulation)
* Letakkan baris chip langsung di bawah kotak input (gabungkan ke dalam satu Auto Layout group dengan gap `6px`).
* Atur `layoutSizingHorizontal = "FILL"`, `layoutSizingVertical = "HUG"`, dan `height = 28px`.
* Seluruh child chip button wajib ber-`layoutGrow = 1` dan nested `._button-base` ber-`layoutSizingHorizontal = "FILL"` untuk mencegah pembengkakan ukuran default (100px) atau teks meluap (*overflow*).
* Sediakan 1 chip batas atas untuk mensimulasikan error validasi:
  - Klik chip nominal normal ➔ `sim_is_invalid = false`.
  - Klik chip over-limit ➔ `sim_is_invalid = true` (border merah + pesan error muncul seketika).

---

## 3. Standard Helpers (Figma Bridge / Plugin API)

```javascript
// Helper 1: Wire Variable-Bound Segmented Control (Tenor / Tabs / Chips)
async function wireSegmentedControls({ buttons, stateVars, dataPayloads }) {
  for (let i = 0; i < buttons.length; i++) {
    const btn = buttons[i];
    const actions = [];

    // 1. Toggle visual states across group
    for (let j = 0; j < stateVars.length; j++) {
      actions.push({
        type: "SET_VARIABLE",
        variableId: stateVars[j].id,
        variableValue: {
          type: "STRING",
          resolvedType: "STRING",
          value: i === j ? "primary" : "outline"
        }
      });
    }

    // 2. Inject data payload
    if (dataPayloads && dataPayloads[i]) {
      for (const [varId, valObj] of Object.entries(dataPayloads[i])) {
        actions.push({
          type: "SET_VARIABLE",
          variableId: varId,
          variableValue: {
            type: valObj.type || "STRING",
            resolvedType: valObj.type || "STRING",
            value: valObj.value
          }
        });
      }
    }

    await btn.setReactionsAsync([{
      trigger: { type: "ON_CLICK" },
      actions: actions
    }]);
  }
}
```

---

## 4. Quality Checklist & Verification Before Completion

1. **Exhaustive Scanning Verification:**
   - Apakah sub-komponen `._field-base` atau `._button-base` sudah diperiksa?
   - Apakah properti `text`, `variant`, `state`, dan `invalid` sudah di-scan sebelum diputuskan mana yang di-bind?
2. **Zero Default 100px Frame Overflow:**
   - Periksa apakah ada frame baru yang secara tidak sengaja berukuran `100×100 px` dengan `layoutSizingVertical = "FIXED"`. Semua wrapper harus ber-`layoutSizingVertical = "HUG"`.
3. **No Duplicate Screen Artboards:**
   - Pastikan tidak ada layar duplikat untuk status tenor atau tab filter di kanvas.
4. **Canvas Geometry & Alignment:**
   - Semua layar alur utama tersusun horizontal rata (`y` seragam, gap `32px`).
   - Master komponen dan bottom sheet diletakkan rapi pada posisinya.
5. **Input Field State Verification (No Dim/Placeholder Text on Filled Inputs):**
   - Periksa seluruh instance `._field-base` yang memiliki nilai teks: pastikan `state === "filled"`, bukan `"idle"`.
6. **Inner-Base Child Sizing Verification:**
   - Periksa seluruh child button di dalam row/grid: pastikan `._button-base.layoutSizingHorizontal === "FILL"` dan tidak ada teks/emoji yang meluap melewati batas parent.
