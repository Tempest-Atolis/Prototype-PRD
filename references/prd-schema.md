# Structured PRD records

Use one structured record per meaningful page-specific requirement. Store records in JSON, YAML, TypeScript, or another project-native structured format; do not require a format solely for the skill.

## Invariants

- `id` is immutable once published and unique across the product.
- `number` is a display-order hint only. The viewer may calculate it from visual order.
- `target.anchor` resolves to one stable DOM element, normally an element `id` or `data-prd-anchor` value.
- A shared component requirement is stored once as a component record; page records reference it.
- Omit optional fields when unknown rather than filling them with invented behavior.

## Canonical shape

```json
{
  "id": "HOME-001",
  "number": 1,
  "target": { "anchor": "search-entry" },
  "title": "Search entry",
  "kind": "interaction",
  "requirement": {
    "description": "Lets a user begin a content search.",
    "interaction": {
      "trigger": "click",
      "action": "navigate",
      "target": "SEARCH"
    },
    "display": { "visibleWhen": "always" },
    "states": ["default", "disabled"],
    "data": null,
    "permission": null,
    "exception": null
  }
}
```

## Record fields

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | Stable requirement ID, e.g. `PAGE-AREA-001`. |
| `target.anchor` | yes for rendered page requirements | Stable DOM anchor without `#`; shared-only records may omit it. |
| `title` | yes | Short product-facing name. |
| `kind` | yes | `display`, `interaction`, `input`, `state`, `rule`, or `data`. |
| `requirement.description` | yes | User-observable responsibility. |
| `interaction` | when interactive | Trigger, action, and optional destination. |
| `display` | when conditional | Visibility condition or audience. |
| `states` | optional | Relevant UI states, including loading, empty, error, or disabled where applicable. |
| `data`, `permission`, `exception` | optional | Only documented when they materially affect behavior. |

## Component records and page references

```json
{
  "id": "COMP-GLOBAL-002",
  "scope": "component",
  "title": "Bottom navigation",
  "requirement": { "description": "Switches between primary product areas." }
}
```

```json
{
  "pageId": "HOME",
  "usesComponents": ["COMP-GLOBAL-002"],
  "requirements": ["HOME-001", "HOME-002"]
}
```

The PRD rail may show component references compactly. It must not duplicate their full requirements on every page.

## PRD-mode rendering guidance

The rail should show a concise title, description, and only populated behavior fields. On a wide review workspace, render it at the far-right edge, outside the prototype canvas; do not place it between sections of a prototype or as an in-canvas middle column. A badge is a navigation affordance, not the requirement identity. Selecting a badge and selecting a rail entry must locate the same record and anchor. Unresolved anchors are visible as an authoring defect, never silently ignored.

For a scrollable prototype, badge geometry must be derived from the target's current bounding rectangle in the same scrolling coordinate space, or the badge must live inside the target's scrolling container. It must update after the container, page, or layout changes scroll. See [PRD-mode layout](prd-mode-layout.md) for the rendering contract.
