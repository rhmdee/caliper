---
name: caliper
description: Use when starting UI/UX design from a PRD, User Story, Acceptance Criteria, or product brief for Figma or FigCraft, or when designing product screens and user flows that require strict Design System compliance and zero guesswork.
---

# Caliper: Brief-to-Design Orchestrator

Single gated pipeline from product requirements to verified Figma or FigCraft screens. Pass each gate in order. Skipping gates, guessing rules, or inventing ad-hoc components is forbidden.

## Precedence & Anti-Conflict Rules

1. **Source Wins:** Written PRD/AC and user confirmations override all assumptions.
2. **Design System Wins:** Target Figma/FigCraft Design System components and variables override generic styling rules.
3. **Caliper Controls Flow:** Helper skills run only as sub-checks inside their assigned gate.
4. **Excluded for Product UI:** Do NOT load `antislop-ui` or `design-taste-frontend`, and do NOT run `antislop` core's install wizard or mode prompt.

## 4-Gate Pipeline

### Gate 1: Brief Extraction & Gap Check
**REQUIRED SUB-SKILL:** Use `product-thinking-uiux`.
- Read external specs via `gsheets` / `gdrive` when linked (if those plugins are installed; otherwise ask the user to paste or export the spec).
- If input is a raw idea with no PRD, optionally use `superpowers:brainstorming` (if installed) or `generate-prd` first.
- **Output:** Verified facts (product type, actors/permissions, core closed-loop flow, KPIs, constraints) + `[Perlu Konfirmasi]` list for UI-impacting gaps only.
- **HARD STOP #1:** Wait for `[Perlu Konfirmasi]` answers, target canvas (`Figma` or `FigCraft`), and target file/Design System before Gate 2.

### Gate 2: Screen Contract & Library Audit
**REQUIRED SUB-SKILL:** Use `ui-end-to-end-flow`.
- **Figma:** Run `figma-design-system-composer` Phase 2 (publish check + component/variable audit; clone if unpublished). Load `figma-use` before any `use_figma` call.
- **FigCraft:** Inspect active components, tokens, and target page via `figcraft`.
- **Output:** Flow Map + **Screen Contract Table** (`Step/ID | Screen & State | DS Components | Core UX Copy | Edge Case`) + Library Audit.
- **HARD STOP #2:** Wait for user approval of the Screen Contract before creating any frame or node.

### Gate 3: Locked Canvas Execution
Build autonomously against the approved Screen Contract:
- **Figma:** **REQUIRED SUB-SKILL:** `figma-design-system-composer` + `figma-use` (add `figma-prototype-composer` only if interactive prototyping is requested).
- **FigCraft:** Apply `ui-end-to-end-flow` + Caliper UI/UX rules (transparent layout wrappers, `*Overlay` scrims using fill opacity, right-bottom Toast, clean end-user copywriting).
- **Copy Filter (Optional):** Apply `antislop-copywriting` principles if installed; otherwise enforce directly that all UI copy is strictly end-user natural (zero `US-xx`/`BR-xx`, FGD notes, or `(contoh)` labels).

### Gate 4: Evidence-Based QA Gate
**REQUIRED SUB-SKILL:** Use `superpowers:verification-before-completion` if installed; otherwise enforce directly: never claim completion without fresh visual evidence and full requirement verification.
- **Figma:** **REQUIRED SUB-SKILL:** `figma-uiux-reviewer` (telemetry + `figma_capture_screenshot` + AC traceability). Optionally add `ux-audit` / `antislop-human` (if installed).
- **FigCraft:** Run `ui-end-to-end-flow` Section 3 checklist + inspect rendered screenshots of every new/edited screen.
- **HARD RULE:** Fix clipped text, overlaps, leaked placeholders, or hardcoded values first. Never report "done" without screenshot evidence and 100% Contract/AC coverage.

## Red Flags (Stop Immediately)

| Temptation | Reality |
| :--- | :--- |
| "I'll fill missing edge cases with common patterns." | STOP. Add to `[Perlu Konfirmasi]` in Gate 1. |
| "I'll sketch a quick frame first." | STOP. Get Screen Contract approval in Gate 2. |
| "I'll draw a raw frame instead of a library component." | FORBIDDEN. Reuse Design System instances only. |
| "Node JSON looks fine, skip screenshot." | FORBIDDEN. Gate 4 requires visual inspection. |
