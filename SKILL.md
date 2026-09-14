---
name: prototype-prd
description: Design, create, or evolve HTML interaction prototypes that expose a clean Preview mode and a structured PRD mode with stable DOM-to-requirement links. Use for Web, admin, responsive Web, H5, APP, mini-program, tablet, and custom HTML prototypes; not for final production UI implementation.
---

# Prototype PRD

Build an inspectable prototype system in which the HTML prototype, structured PRD records, and PRD viewer describe the same product behavior without making either representation an opaque copy of the other.

## Scope and outcome

Use this skill for HTML-based interactive prototypes and their requirement documentation. Preserve the project's existing framework and visual language. The deliverable must keep a clean stakeholder-facing Preview mode and provide a review-facing PRD mode where a reviewer can navigate between visible prototype anchors and structured requirement records.

This is not a mandate to build a production application, replace an existing design system, or claim fully automatic bidirectional synchronization before a runtime and change-detection mechanism exist.

## Start with the project

Before changing a prototype, inspect its project instructions, current page structure, routes, existing shared components, and any existing PRD source. Reuse stable DOM IDs or explicit `data-prd-anchor` attributes; do not bind requirements to fragile CSS selectors when a stable anchor is available.

If the project has no configuration, propose or create one only when the user asks to initialize or implement the prototype system. Determine the platform profile from the product context; a profile supplies defaults, never a fixed viewport requirement. Read [platform profiles](references/platform-profiles.md) when selecting or defining a profile.

## Core model

Treat these as distinct but linked sources:

```text
Prototype HTML  <->  stable DOM anchor  <->  requirement record  ->  PRD mode viewer
```

- A requirement has an immutable semantic ID such as `HOME-001`; visual numbers are derived presentation labels and may change.
- A page references shared components rather than duplicating their requirements or markup.
- A requirement record states product behavior, not merely what happens to be drawn today.
- Preview mode must not leak annotation badges, connectors, editor controls, or the PRD rail into a normal prototype view.

Read [PRD schema](references/prd-schema.md) before creating or changing requirement records. Read [PRD-mode layout](references/prd-mode-layout.md) before implementing annotations or a PRD viewer. Read [PRD editing](references/prd-editing.md) before adding editable PRD records. Read [shared architecture](references/shared-architecture.md) when introducing shells, common components, viewer runtime, or future synchronization.

## Choose the appropriate operation

### Design or initialize

Establish the platform profile, viewport policy, shared shell, component boundaries, page IDs, requirement-ID convention, PRD storage location, and output language. Keep configurations and field names in English; generate requirement prose and visible UI text in the project's language.

### Add or revise a page

Keep page-specific business UI in the page. Reference the shared shell and common components. Identify only meaningful business, interaction, state, or rule-bearing elements for PRD anchors; do not annotate every decorative container.

### Add PRD mode

Render badges and a PRD rail from structured records, rather than hard-coding prose into page markup. On a wide workspace, the PRD rail belongs at the far right as an independent overlay or reserved external region. Entering PRD mode MUST NOT resize, shift, scale, or otherwise change the prototype canvas position relative to Preview mode. Support two-way navigation: selecting an anchor focuses its record, and selecting a record focuses its anchor. Badges must follow their target elements while the prototype page scrolls. Connectors, overlays, and hover outlines are optional presentation aids and must remain isolated from Preview mode.

### Add Edit PRD mode

When editing is requested, provide an Edit button on each PRD record and expand its editable text fields in that record's own card. Do not route every edit through a shared editor placed below the full list. Validate the record and its anchor before committing, and make unsaved state, save, cancel, and failure visible to the reviewer. Requirement IDs and DOM anchors are mapping keys, not ordinary editable prose. When a record declares an explicit HTML text binding, offer a separate, clearly named Save and apply to HTML action that updates only that binding; ordinary Save changes only the structured PRD source.

### Synchronize or export

Use explicit mappings and inspect changes before proposing updates. A changed visible label alone is not sufficient evidence to rewrite a requirement: classify whether it changes product behavior, interaction, rule, or only presentation. Never silently overwrite HTML or PRD records. Markdown exports are derived artifacts unless the project explicitly designates them as the authoritative PRD.

## Non-negotiable design rules

- Global headers, sidebars, navigation bars, tab bars, and footers MUST be shared components when repeated in a product.
- Repeated UI markup MUST NOT be copied independently into each page when the project can reuse a shell or component.
- The skill MUST NOT prescribe a specific visual style, navigation count, or device dimension.
- Desktop and responsive pages must support their stated viewport strategy; mobile-oriented pages may use a fixed artboard, content-driven height, or both as configured.
- Keep annotations and PRD runtime framework-agnostic unless the existing project requires an adapter.
- Add an anchor only after the target element exists and has a stable identity.
- On desktop, keep the PRD rail at the right edge of the review workbench; on narrow screens, replace it with a dedicated PRD view or an explicit right-side/bottom sheet rather than covering the prototype.
- Do not position a page annotation relative to a static phone frame or viewport unless its target is also fixed. An annotation for scrollable content MUST move with that content or be recalculated on each relevant scroll event.
- In Edit PRD mode, preserve stable requirement IDs and anchor mappings; do not silently change or delete either through ordinary field editing.
- Never write edited PRD values into prototype HTML as an implicit side effect of Save.
- PRD mode MUST preserve the Preview-mode prototype canvas geometry. The PRD rail may overlay unused workbench space, but it must not cause the canvas to reflow or move.

## Completion check

Report the chosen platform/profile, files or artifacts changed, source-of-truth decision, preview/PRD behavior verified, and any deferred synchronization capability. For implementation work, verify Preview mode remains visually clean and that each rendered requirement can resolve its target or is explicitly reported as unresolved.
