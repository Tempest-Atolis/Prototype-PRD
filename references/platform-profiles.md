# Platform profiles and viewport policy

A profile establishes starting defaults for a prototype project. It does not impose a visual design or prevent a project from overriding its viewport, shell, navigation, or breakpoint policy.

## Suggested profiles

| Profile | Starting viewport policy | Typical shared shell |
| --- | --- | --- |
| `desktop-web` | 1440 x 900 artboard by default | Header, optional footer |
| `admin` | 1440 x 900 artboard by default | Header, sidebar, breadcrumb |
| `responsive-web` | Multiple breakpoints, commonly 1440/1024/768/390 | Responsive header and content layout |
| `h5` | 390 x 844 starting artboard | Mobile navigation as configured |
| `app` | 390 x 844 starting artboard | App bar and optional tab bar |
| `mini-program` | 390 x 844 starting artboard | Platform navigation and optional tab bar |
| `tablet` | 1024 x 1366 starting artboard | Tablet-specific header/navigation |
| `custom` | Explicit project values | Defined by the project |

The dimensions are recommendations, not standards. A project may select a fixed height, content-driven height, a different device size, or a multi-breakpoint matrix.

## Project configuration example

```yaml
project:
  name: Example Product

platform:
  type: mini-program

viewport:
  width: 440
  minHeight: 956
  heightMode: content

shell:
  navigationBar: true
  bottomNavigation: true
  sideNavigation: false

components:
  reuse: required

prd:
  language: zh-CN
  mode: side-panel
  numbering: automatic
  connectors: optional
```

For responsive projects use an explicit list rather than implying that a desktop artboard is responsive:

```yaml
platform:
  type: responsive-web
viewports: [1440, 1024, 768, 390]
```

## Selection criteria

Choose by the interaction and delivery context, not by a preferred pixel size. Use `custom` when a platform preset would conceal a relevant device, kiosk, embedded, or fixed-display constraint. Keep the profile selection in project configuration so it can be inspected and changed without modifying the skill.
