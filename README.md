# Prototype PRD

A Codex Skill for HTML interaction prototypes. It adds a clean Preview mode and a structured, review-oriented PRD mode without turning the prototype into a production application.

It supports desktop Web, admin systems, responsive Web, H5, app, mini-program, tablet, and custom-size prototypes.

Maintained by [QiuYue](https://github.com/Tempest-Atolis).

## Capabilities

- Links prototype elements to Requirement IDs through stable DOM anchors.
- Renders navigable annotations and a far-right PRD rail without changing the prototype canvas geometry from Preview mode.
- Keeps annotations attached to their target DOM elements while a page scrolls.
- Requires repeated product chrome, such as headers, sidebars, tab bars, and footers, to use shells or shared components.
- Supports per-card editing: every PRD record has its own Edit button and expands its fields in place rather than using an editor at the bottom of the list.
- Supports explicit HTML text bindings. A page label can be updated only when a record declares one unique `htmlBinding` and the reviewer chooses the explicit apply-to-HTML action.

## Structure

```text
prototype-prd/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── platform-profiles.md
    ├── prd-schema.md
    ├── prd-mode-layout.md
    ├── prd-editing.md
    └── shared-architecture.md
```

## Installation and use

Place this directory in the Codex Skills directory, for example:

```text
%USERPROFILE%\\.codex\\skills\\prototype-prd
```

Then prompt Codex with a request such as:

```text
Use prototype-prd to add Preview and PRD modes to the existing HTML prototype.
```

For editable requirements:

```text
Add in-place editing to the PRD cards in the right rail. For records with an htmlBinding, provide an explicit Save and apply to HTML action for the mapped page label.
```

## Key boundaries

- Requirement IDs and DOM anchors are protected mapping keys, not ordinary editable text.
- Ordinary Save PRD updates only the structured requirement source.
- Save and apply to HTML is explicit and may update only one validated, unique HTML text binding.
- A standalone static demo can update the live DOM in the browser. Persisting changes to the actual HTML source file requires a project runtime or an explicit export/writeback mechanism.

See [SKILL.md](SKILL.md) and the files in `references/` for the complete rules.

## License

This project is licensed under the [MIT License](LICENSE).
