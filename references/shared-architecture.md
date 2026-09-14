# Shared architecture and evolution path

## Component boundary

Use three layers:

```text
Platform shell
├─ shared components
└─ page content
   └─ page-specific components
```

The platform shell owns repeated product frame elements, such as headers, sidebars, navigation bars, tab bars, and footers. Shared components own reusable controls such as buttons, search, modals, empty states, cards, and form fields. Pages own business-specific composition and components.

Do not centralize a component merely because two elements look alike once; centralize it when it represents the same product behavior or is expected to change together. Conversely, shared markup must not be copied page by page merely to avoid defining a boundary.

## Runtime separation

The prototype runtime should remain separate from product UI:

```text
product page + shared shell
           |
           +-- stable anchors
                    |
structured PRD source -- PRD runtime -- badge/overlay/rail
```

The runtime may be framework-agnostic JavaScript and CSS, with optional framework adapters later. It must be disableable or absent in Preview mode. It should not require business markup to contain generated annotation controls.

## Source-of-truth policy

At project setup, explicitly choose the source of truth:

- **Structured PRD first:** HTML is an implementation/visualization of requirements.
- **Prototype first:** structured PRD is reviewed and reconciled from HTML changes.
- **Controlled dual source:** updates require a reviewable change set before either side is modified.

Avoid presenting a Markdown export as authoritative unless the project states that it is. Otherwise it is derived from structured records.

## Incremental bidirectional synchronization

Implement in stages:

1. **Linked viewing:** anchors and structured PRD records render reliably in PRD mode.
2. **Change detection:** compare anchor existence, visible labels, and relevant attributes; classify possible requirement impact.
3. **Reviewable reconciliation:** generate a proposed PRD or HTML change with an explicit mapping and user approval.
4. **Targeted writeback:** apply approved changes only to mapped fields/elements; retain a change record.

Visible text changes should be treated as candidates for review, not automatic product-rule changes. A structural DOM difference is not proof of a changed requirement.

## Suggested future implementation boundary

When implementation begins, a TypeScript runtime is suitable because it can inspect DOM, bind events, render the rail, and observe prototype changes. Keep it framework-neutral initially; adapters are justified only by a concrete project need. Deterministic operations such as scanning anchors, validating IDs, rendering exports, and producing diffs may later be added as scripts.
