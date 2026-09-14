# Edit PRD mode

Use this reference when a reviewer must change requirements from the PRD rail.

## Editing boundary

Edit mode changes the structured PRD source. It does not directly modify HTML, shared components, navigation, or visual design unless the record declares a specific HTML text binding and the reviewer explicitly chooses the separate apply-to-HTML action.

```text
right-side PRD editor -> validate -> structured PRD source
                                         |
                                         +-> optional reviewed HTML change proposal
```

## Editable and protected fields

Expose fields only when they are meaningful for the record. Typical editable fields are:

- `title`
- `kind`
- `requirement.description`
- `requirement.interaction`
- `requirement.display`
- `requirement.states`
- `requirement.data`, `permission`, and `exception`

The following are protected mapping fields in ordinary edit mode:

- `id`: immutable after the record is published.
- `target.anchor`: changing it can break prototype linkage.
- `scope`: changing page/component ownership is a structural operation.

If the user explicitly requests a mapping change, present the old and proposed anchors, validate the target exists, and require a deliberate confirmation before commit. If the target is missing, leave the record unresolved rather than guessing a replacement.

## Editor placement and behavior

- Keep edit controls in the far-right PRD rail; do not make the prototype canvas itself a rich-text editor.
- Every editable PRD card has its own Edit button. Selecting it expands text fields inside or directly below that card, so the reviewer can see the requirement context while editing.
- Do not place a single shared editor after the complete list: it forces long-distance scrolling and breaks the relationship between the selected record and its fields.
- Entering edit mode loads a draft for that record. The viewer continues to show the last saved record until the draft is saved.
- Mark changed records as unsaved. Provide Save and Cancel for the current draft; provide a clear failure message when validation or persistence fails.
- Save validates required fields, the stable ID, and anchor resolution for page-bound records. A failed save preserves the draft.
- After a successful save, rerender the rail and update the selected record. Reposition or refresh any affected annotation labels.
- Use ordinary form controls for structured fields. Freeform text is appropriate for descriptions; use constrained controls for `kind`, state values, and interaction action where the project has a controlled vocabulary.

## Explicit HTML text binding

An editable record may declare a narrow text mapping when a specific PRD field represents a visible page label:

```json
{
  "htmlBinding": {
    "field": "uiText",
    "selector": "#quick-ledger [data-ui-text]",
    "property": "textContent"
  }
}
```

Render the mapped `uiText` as a normal text input in the same card. Provide two distinct actions:

- **Save PRD:** persists only the structured requirement record.
- **Save and apply to HTML:** persists the record and updates only the declared selector/property mapping after validating that exactly one target resolves.

Never infer an HTML target from a record title or description. If the binding is absent, unresolved, or matches more than one element, disable the apply action and show the reason. For a standalone browser demo, applying to HTML updates the live preview DOM; source-file persistence requires a project runtime or an explicit export/writeback mechanism.

## Change classification

On Save, classify the change before offering follow-up actions:

| Change | Result |
| --- | --- |
| Description only | Save PRD; no HTML change proposed by default. |
| Interaction, display, state, data, permission, or exception | Save PRD and flag that the prototype may need review. |
| Mapped user-visible label | Save PRD; expose the separate Save and apply to HTML action. |
| ID, anchor, or scope | Treat as structural; require explicit mapping review and confirmation. |

## Persisting and errors

Use the project's established PRD storage mechanism. If the page is a standalone demonstration without a writable structured source, make Save visibly unavailable or store only a clearly labeled local draft; do not pretend a file was updated. Preserve the original record until a valid save completes.

## Verification cases

Verify at minimum:

1. Edit and save a description: the structured record and rail update while prototype HTML stays unchanged.
2. Cancel a draft: the saved record remains unchanged.
3. Attempt an invalid or missing anchor: save is rejected and the draft remains visible.
4. Change an interaction rule: the rail marks the prototype as needing review rather than silently editing it.
