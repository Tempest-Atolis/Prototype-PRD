# PRD-mode layout and scrolling contract

Use this reference when implementing a PRD rail, annotations, connectors, or their responsive layout.

## Desktop workbench

The prototype canvas and PRD rail are separate workbench regions:

```text
review toolbar | prototype canvas / device frame | flexible empty workspace | PRD rail
                                                                    right edge
```

The PRD rail belongs at the far right of the overall review workspace, not immediately inside a phone frame and not between the prototype's page content and its device chrome. It should remain independently scrollable and visible while the reviewer inspects a long prototype. Showing or hiding it MUST NOT change the prototype canvas's width, scale, alignment, or position: the Preview-mode and PRD-mode canvas occupy the same geometry.

Use a fixed or overlay rail when Preview and PRD mode share the same canvas placement. A separately reserved right-side workbench region is also valid only when it is reserved in both modes. Do not introduce a PRD-only grid column that shifts the canvas. Reserve sufficient right-side workspace so the rail does not overlap the prototype. The rail may have a bounded readable width; its outer position, rather than a large width, establishes the far-right placement.

On narrow viewports where a side rail would make the prototype unreadable, do not force a three-column layout. Use an explicitly opened side sheet, bottom sheet, or a separate PRD view. The selected PRD record and target focus must remain synchronized across that transition.

## Annotation coordinate rule

Each badge represents a target DOM element, not a coordinate in a device frame.

Choose one of these rendering strategies:

1. **In-container badge:** place the badge in the target's nearest scrollable annotation layer. That layer scrolls with the page content.
2. **Measured overlay:** render badges in a separate overlay but calculate each position from `target.getBoundingClientRect()` relative to the overlay's current rectangle. Recalculate on every relevant scroll, resize, content mutation, font load, and mode activation.

Never use a one-time `top`/`left` value tied to a static phone frame for an element inside a scrollable page. Fixed targets, such as a fixed tab bar, may use fixed-coordinate badges only when their fixed relationship is intentional.

## Interaction and focus

- Selecting a badge scrolls its target into an appropriate visible position before applying focus styling.
- Selecting a PRD record scrolls and focuses the target, then updates the selected badge/record state.
- After any programmatic scroll finishes, update measured overlays before drawing connectors or focus outlines.
- When a target is clipped, hidden, or absent in the active state, show the record as unresolved or state-bound; do not leave a stale badge in its last position.

## Verification cases

Verify each PRD-mode page at minimum:

1. Scroll the prototype from top to bottom: every scrollable-content badge stays attached to its target.
2. Select an item near the bottom of the PRD rail: its target becomes visible and its badge remains aligned.
3. Resize between desktop and narrow layouts: the rail moves to the configured alternate view without overlaying the prototype.
4. Toggle Preview mode: the rail, badges, overlays, and reserved review-only spacing disappear.
