---
name: ux-audit
description: Reviews a Figma design or user flow for UX heuristics, behavioral logic, accessibility, and copywriting clarity.
---

# Audit UX Heuristics & Flow

Review a Figma node, screen, or user flow for evidence of poor UX practices, broken user flows, confusing copywriting, or accessibility issues. 

This skill is read-only. It acts as a Senior UX Researcher & Designer. When the user wants a write action afterward, downstream skills should use `fix-ux-finding` or `apply-ux-improvements`.

## Output Format Selection

- **Explicit user request wins**: If the user asks for `--json`, output raw JSON. If `--markdown`, output markdown.
- **Machine-consumed review surfaces**: Output raw JSON by default.
- **Chat-style environments**: Output the human-readable markdown report by default.

## Workflow

1. **Parse the Figma input.**
   Accept a full Figma URL, or a `fileKey` and `nodeId`. 

2. **Pull the required evidence with Figma MCP read tools.**
   Call `get_design_context` for the node under review.
   Call `get_screenshot` for visual layout, typography hierarchy, and contrast evaluation.
   Call `get_metadata` to understand the broader user journey (e.g., previous/next screens in the flow).

3. **Evaluate against UX Standards & Heuristics.**
   Review the design through 4 main lenses:
   - **User Flow & Behavior**: Are there dead ends? Is the primary action obvious? Are error states and empty states accounted for? Is the cognitive load too high?
   - **Jakob Nielsen's 10 Usability Heuristics**: E.g., Visibility of system status, Match between system and the real world, User control and freedom, Consistency.
   - **Copywriting & Microcopy**: Is the text concise, clear, and action-oriented? Do buttons start with verbs? Is the tone consistent?
   - **Accessibility (A11y)**: Are touch targets large enough (min 44x44pt)? Is text contrast sufficient (WCAG AA)?

4. **Suggest Actionable Fixes.**
   For every finding, provide a concrete recommendation on how to fix it structurally or textually. Do not just point out the problem; provide the UX solution.

5. **Present findings in the appropriate format.**
   Use JSON for machines, markdown for chat environments.

6. **Route to downstream skills.**
   Suggest `fix-ux-finding` to repair specific text or layout issues.
   Suggest `rebuild-user-flow` if the entire screen logic needs restructuring.

## What To Flag

- **Friction Points**: Too many steps, unnecessary inputs, or hidden primary calls-to-action (CTAs).
- **Dead Ends**: Modals without a clear close button, confirmation screens without a way back to the dashboard.
- **Confusing Copywriting**: Jargon, passive voice, or vague button labels (e.g., "Submit" instead of "Save Profile").
- **Missing States**: Lack of loading, empty, success, or error states.
- **Visual Hierarchy Failures**: Secondary actions competing visually with primary actions.
- **Accessibility Violations**: Low contrast text, tiny touch targets.

## What Not To Flag

- Purely aesthetic UI preferences (e.g., "I prefer rounded corners").
- Systemization issues (leave this to `audit-design-system`).
- Code implementation details (unless it impacts perceived performance/UX).

## Evidence Standard

Every finding must answer:
1. **What is the issue?** (e.g., "The 'Cancel' and 'Delete' buttons have the same visual weight.")
2. **Why does it matter?** (e.g., "Violates error prevention heuristics; users might accidentally delete data.")
3. **How to fix it?** (e.g., "Make 'Delete' a destructive primary button, and 'Cancel' a ghost/text button.")

## Output Format

### Human-Readable Markdown Report

When the selected output format is markdown, present a formatted markdown report with:

1. **Header section:**
   - Flow/Screen being reviewed
   - Overall UX Health: ✅ Excellent / ⚠️ Needs Refinement / ❌ High Friction
   - Usability Score (0-100)

2. **Executive Summary:** 2-3 sentences explaining the core UX bottlenecks.

3. **Findings Table:** Quick overview with priority indicators
   - 🔴 Critical (Priority 3): User cannot complete the core task, severe accessibility failure.
   - 🟠 High (Priority 2): High friction, confusing logic, likely to cause errors.
   - 🟡 Medium (Priority 1): Suboptimal microcopy, minor cognitive load issues.
   - ⚪ Low (Priority 0): Minor polish, nice-to-have UX enhancements.

4. **UX Findings Details:** Expand each finding with:
   - **Observation:** What is currently happening.
   - **UX Impact:** Why it's bad for the user (cite specific heuristics).
   - **Recommendation:** Actionable fix (new copy, new layout logic).
   - Affected node IDs.

### JSON Output (for machine consumption)

```json
{
  "ux_findings": [
    {
      "title": "<= 80 chars, imperative>",
      "category": "Flow | Copywriting | Accessibility | Heuristics",
      "observation": "<What was found>",
      "ux_impact": "<Why it hurts the user>",
      "recommendation": "<How to fix it>",
      "priority": 0,
      "code_location": {
        "absolute_file_path": "/figma/<fileKey>/nodes/<nodeId>"
      }
    }
  ],
  "overall_ux_health": "High Friction",
  "usability_score": 65
}
```

## Example Trigger Phrases
- "Audit the checkout flow for UX friction"
- "Review this screen's copywriting and error states"
- "/ux-audit https://figma.com/design/..."
