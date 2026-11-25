# Design System Specific Notes & Quirks (Optional Reference)

Consult this reference **only after** Phase 2 Dynamic Discovery identifies which Design System is active in the current file or team library. Always trust live discovered IDs, properties, and tokens over static notes.

---

## 1. Unified Pillar-UI v2 Contract (Bento, Nexus v2, Enterprise v2)

All three official **Perkasa Pilar Utama** Design Systems share the same **3-page Figma Starter-compatible structure** (`Cover`, `Components`, `UI Flow`), **5 Variable Collections** (`00 · Tailwind Primitives` hidden, `01 · Brand` hidden, `02 · Semantic · shadcn/ui` `Light`/`Dark`, `03 · Enterprise Extensions` `Light`/`Dark` + 5 RGBA glass/scrim tokens, `04 · Layout & Radius` `Desktop`/`Mobile`), **11 Text Styles**, **7 Effect Styles**, **Native Slots**, **100% Skeleton parity**, **WCAG 2.1 AA (`>= 5.16:1`)**, and **0 `._base` wrappers**.

- **Shared Rules Across All 3 Presets:**
  - **BrandLogo:** Default to official Perkasa Pilar Utama `BrandLogo` (`showTagline=true` only at `size=lg`; `showTagline=false` in compact headers; `type=mark` at `<= 28px`).
  - **Toast:** Direct child of root screen with `absolute: true` (bottom-right `24px`, no `ToastViewport`). `success`/`info`/`warning` omit `×`; `error` MUST include `×`.
  - **Cards:** NEVER use thick colored left-border accent stripes (`border-l-2`/`border-l-4` colored bars) to indicate priority or status; use semantic `Badge` or clean surface hierarchy instead.
  - **Code Registry Slug:** Every `PascalCase` Component Set maps 1:1 to `@pillar/<kebab-case>` in the `pillar-ui-ds` custom registry (e.g., `TableFull` → `@pillar/table-full`, `DealCard` → `@pillar/deal-card`, `DescriptionList` → `@pillar/description-list`).

---

## 2. Pillar-UI: Bento (`v1.0.0` — Executive Dashboard & Analytics)

- **Typography & Radius:** `Inter` + `JetBrains Mono`; base radius `12px` (`radius/2xs: 4px` .. `radius/3xl: 24px`, `radius/pill: 9999px`).
- **Palette Signature:** Soft Slate outer canvas (`#F1F5F9` Light / `#020618` Dark) with white/navy Bento cards, Perkasa Blue `primary` (`#104D9C`/`#266CC9`), and Perkasa Green `brand` (`#36712E`, `secondary/700`).
- **Shell & Mobile:**
  - **Desktop (`1360×860`):** `AppLayout` (`HeaderActions`, `Content` slots) + Floating Bento Left Rail; `OverlayContainer` (`type=dialog|sheet`, floating right `Sheet` `p-3` without drag handle).
  - **Mobile (`390×844`, Mode `Mobile`):** `MobileAppLayout` + floating **Frosted Glass Dock** `MobileBottomNav` (`rounded-full`, equal `6px` X/Y padding, `surface-glass` + `Surface/GlassDock` blur `14px`).
- **Signatures:** 5 `CardPattern/*` vectors (`BrandCurve`, `DotSpectrum`, `RotatedGrid`, `PillarBars`, `RadarRings`), `62` Component Sets.

---

## 3. Pillar-UI: Nexus (`v2.0.0` — Modern SaaS, CRM & AI Workspace)

- **Typography & Radius:** `Geist` + `Geist Mono`; base radius `10px` (`radius/2xs: 2px` .. `radius/3xl: 22px`, `radius/pill: 9999px`).
- **Palette Signature:** Pure `neutral` scale (`#FFFFFF`/`#F5F5F5` Light; `#0A0A0A`/`#262626` Dark) aligned 1:1 with `shadcn` `base-nova` + `@base-ui/react`.
- **Shell ("The White Box") & Mobile:**
  - **Desktop (`1440×900`):** Outer canvas `accent` (`p-2`, `gap-1.5`) + collapsible `FloatingLeftRail` (`256px` expanded / `72px` collapsed, `rounded-2xl`) + White Box (`rounded-2xl`, `TopHeaderBar` `56px`). Floating Right `Sheet` (`p-3`, `rounded-2xl`, no drag handle).
  - **Mobile (`390×844`, Mode `Mobile`):** `MobileAppLayout` + floating **Frosted Glass Dock** `MobileBottomNav` (`rounded-full`, equal `6px` X/Y padding).
- **Signatures:** `DealCard` (CRM Kanban `default|priority` without thick left border), `Checkbox`/`RadioGroupItem`/`Switch` with `style=inline|card`, `Avatar` `status=online`, AI Workspace (`MessageBubble`, `Trace`, `CodeBlock`, `Attachment`, `Questionnaire`), `63` Component Sets.

---

## 4. Pillar-UI: Enterprise (`v2.0.0` — Industrial ERP, Finance & Backoffice)

- **Typography & Radius:** `Plus Jakarta Sans` + `JetBrains Mono` (**NEVER** use `Exo 2` in UI text); ultra-sharp **`0px` radii** (`radius/2xs`..`radius/3xl: 0px`), with `radius/pill: 9999px` reserved for pill badges, switches, radio buttons, and avatars.
- **Palette Signature:** Crisp hairline borders (`#D4D4D4` Light / `#404040` Dark), **Charcoal Industrial** secondary (`#404040` / `#FFFFFF`), and True Dark Mode (`#0A0A0A` / `#171717`).
- **Badge Rule (CRITICAL):**
  - Use **`Badge (style=pill)`** (`9999px` full-rounded) outside tables (KPI cards, document headers, filters) for dynamic contrast against `0px` geometry.
  - Use **`Badge (style=plain)`** (bare `6px` dot + text, no fill/border wrapper) **inside dense ERP table rows** so `34px` compact rows never feel cramped.
- **Shell & Mobile (CRITICAL):**
  - **Desktop (`1440×900`):** Rigid flush `AppLayout` + **Flush Right `Sheet`** (`p-0`, `0px`, full height, no drag handle).
  - **Mobile (`390×844`, Mode `Mobile`):** **`MobileBottomNav` Flush Industrial Bar (`0px` full-width, `56px` height, `1px` top border, `2px primary` top active indicator)** — **DO NOT** use floating Frosted Glass Dock on Enterprise.
- **6 ERP Signatures (`68` Component Sets):** `TableFull` (`TableRow state=compact` `34px` + checkbox + `state=summary` Grand Total), `PrintPreview` (A4 Portrait), `Stepper` (horizontal approval), `DescriptionList` (`columns=2|4`), `Comparison` (COA tree/diff), and `Timeline`.

---

## 5. Legacy Archive: Pillar-UI Nexus v1 (Deprecated Nested-Base Files Only)

*Only applies if opening an unmigrated legacy Nexus v1 file where Phase 2 discovery detects `._field-base` / `._button-base` and `has-modal` on `layout`:*
- Clamp outer `layout` height to `layoutInst.minHeight || 768` when `has-modal = true`.
- Switch nested `._field-base` to `state="filled"` when populating input text; set `._button-base.layoutSizingHorizontal = 'FILL'` inside fixed-width cells; nullify `itemInst.minWidth = null` on `card-item` (`384px` hardcoded) inside multi-column grids.
