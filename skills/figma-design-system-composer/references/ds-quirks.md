# Design System Specific Notes & Quirks (Optional Reference)

Consult this reference **only after** Phase 2 Dynamic Discovery identifies which Design System is active in the current file or team library. Always trust live discovered IDs, properties, and tokens over static notes.

---

## 1. Pillar-UI: Bento (Modern Tailwind v4 + Base UI Standard)

- **Canvas Structure (3-page Starter-compatible layout):** `Cover`, `Components` (7 category columns), `UI Flow`.
- **Variable Collections & Modes:**
  - `00 · Tailwind Primitives` (`Value`, hidden from publishing)
  - `01 · Brand` (`Perkasa Pillar`, hidden from publishing)
  - `02 · Semantic · shadcn/ui` (`Light`, `Dark`) — e.g. `background`, `foreground`, `card`, `primary`, `secondary`, `muted`, `destructive`, `border`, `ring`
  - `03 · Enterprise Extensions` (`Light`, `Dark`) — e.g. `brand`, `heading`, `surface-dark`, `surface-featured-from/to`, `surface-glass`, `border-glass`, `brand-glass`, `brand-glass-border`, `surface-scrim`, `success/warning/info/destructive-*`
  - `04 · Layout & Radius` (`Desktop`, `Mobile`) — `space/0`..`space/8`, `radius/2xs`..`radius/3xl`, `radius/pill`
- **Typography & Effects:**
  - 11 Local Text Styles using **`Inter`** (`Metric/*`, `Display/*`, `Heading/*`, `Body/*`, `UI/*`) and **`JetBrains Mono`** (`Code/Block`, `Code/Inline`).
  - 7 Local Effect Styles: `Shadow/xs..lg`, `Ring/Focus`, `Ring/Error`, `Surface/GlassDock`.
- **Shell & Overlay Pattern:**
  - **Desktop (`1360×860`):** `AppLayout` (`state=default|skeleton`, Native Slots `HeaderActions` & `Content`). Overlays use `OverlayContainer` (`type=dialog|sheet`, `absolute: true`, bound to `surface-scrim`).
  - **Mobile (`390×844`):** Apply `04 · Layout & Radius` Mode `Mobile` on root screen; use `MobileAppLayout` (`state=default|skeleton`, Native Slot `Content`, floating frosted-glass `MobileBottomNav`). Bottom sheet overlays use `DrawerOverlay` (`surface-scrim`) + `Drawer (side=bottom)`.
  - **Toast:** Direct child of root screen with `absolute: true` (bottom-right `24px`, no `ToastViewport`). Error Toast includes `×`; success/info/warning omit `×`.
  - **Skeleton Parity:** Every component set provides a `skeleton` variant; use it for loading states instead of drawing ad-hoc grey boxes.
  - **BrandLogo Rule:** Enable `showTagline` only at `size=lg`; keep `showTagline=false` in compact headers (`TopHeaderBar`) and use `type=mark` for `≤28px` placements.

---

## 2. Pillar-UI: Nexus (Legacy Nested-Base Architecture)

- **Shell & Modal Quirk:**
  - Uses master shell `layout` with nested `main` → `content` slot and boolean modal prop `has-modal`.
  - **Viewport Height Clamping:** When `has-modal = true` is activated on `layout`, clamp outer height to `layoutInst.minHeight || 768` so the backdrop scrim covers 100% edge-to-edge without bottom cutoff.
- **Nested Base Sub-Instances:**
  - Form fields use `field` wrapping nested `._field-base` (`state`: `"idle"` vs `"filled"` — always switch `._field-base` to `"filled"` when populating text values so text does not render as dimmed placeholder).
  - Buttons wrap nested `._button-base` (`type`: `"no-icon"` | `"leading-icon"` — set `._button-base.layoutSizingHorizontal = 'FILL'` inside fixed-width rows/grids).
  - Cards wrap `card` → `card-content` slot → `card-item` (`minWidth: 384px` hardcoded — ALWAYS set `itemInst.minWidth = null` inside multi-column grids and deduplicate vertical padding).
- **Typography & Token Naming:**
  - Uses **`Geist`** font family, `font-size/core/*`, `space/core/*`, `radius/core/*`, and `default/*` / `muted/*` color variable paths.
