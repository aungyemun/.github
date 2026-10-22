# User Interface and User Experience Design

Engineering policy for authorized UI work: structure, layout, typography, multilingual
text, colour, controls, responsiveness, platform conventions, accessibility, UI
performance, copyright and licence presentation, and verification. Visual identity,
logos and application icons are covered by [LOGO_AND_BRANDING](LOGO_AND_BRANDING.md).
Responsiveness mechanics are in [PERFORMANCE](PERFORMANCE.md) section 6, UI testing
discipline in [TESTING](TESTING.md), architecture in
[DEVELOPMENT](DEVELOPMENT.md).

There is no house style. Each application keeps its own purpose, users, framework,
languages, platforms, licence and visual identity; this policy sets quality standards,
not a common look. Never redesign an existing interface beyond what the task asks for,
and never remove working features to improve appearance.

## 1. Understand the application first

Before designing or changing an interface, establish: its purpose and intended users;
the existing UI architecture and framework; supported operating systems and interface
languages; the main workflows; technical complexity; accessibility needs; the existing
visual identity; performance constraints; and project requirements. Apply only what is
relevant - typical needs differ:

| Application type | Typical elements (only where the application needs them) |
| --- | --- |
| Scientific analysis | data tables, workflow stages, advanced configuration, correct scientific terminology, processing progress, validation messages, reproducibility information (versions, parameters), result inspection |
| PDF or document tools | document canvas, navigation, editing tools, zoom, properties inspector, save/export, file operations |
| Personal database | search, records, categories, forms, filters, data-management controls |
| Simple utilities | a compact main window, a few clearly labelled controls, progress feedback, basic settings |

Avoid panels, controls and complexity the application does not need.

## 2. Respect the UI framework

Detect the actual technology (Qt/PySide/PyQt, Electron, Tauri, GTK, Tkinter, .NET
WPF/WinForms/WinUI, native Windows or macOS, web) and follow its conventions, layout
managers, styling and accessibility APIs. Never replace the framework to satisfy this
policy; a framework replacement is an architectural change that requires explicit
authorization.

## 3. Typography

- Use one coherent primary UI font family per script, normally the platform's system UI
  font, with proper fallbacks; avoid mixing families without reason.
- Define a size and weight hierarchy (title, section heading, label, button, table,
  tooltip, description, warning, status) and use it consistently across windows. Sizes
  differ by role; do not force one size everywhere and do not change sizes randomly
  between sections.
- Keep comfortable line height and spacing; prevent clipped or truncated labels; respect
  the user's DPI and font-scaling settings instead of fixed pixel fonts.
- Display and preserve Unicode file names and user data exactly.

## 4. Multilingual interfaces

Support only the languages the application's requirements include; never enable extra
languages on your own initiative.

- **Fonts per script.** Choose fonts by script coverage, glyph quality, readability,
  platform availability, fallback behaviour, Unicode handling, licence and
  redistribution rights, performance and consistency with the design. No single font
  renders every writing system well: do not force a Latin-oriented font onto Japanese,
  Chinese, Korean, Myanmar, Arabic, Hebrew or other scripts when it renders them poorly;
  prefer platform-native fallback (e.g. Yu Gothic or Meiryo on Windows and Hiragino on
  macOS for Japanese; Noto families where bundling is justified).
- **Complex text.** Rely on the framework's text engine for shaping, combining
  characters, bidirectional text, line breaking, wrapping, fallback, input methods,
  cursor movement, selection, copy and paste, and accessible names. Do not write custom
  shaping. Correct storage of Unicode does not prove correct rendering - look at it.
- **Layout adaptation.** Plan for text expansion and different conventions: flexible
  label widths, wrapping, correct direction and mirrored layouts for right-to-left
  languages, dynamic sizing, locale-aware dates and numbers where relevant, working input
  methods, no hard-coded English text dimensions, and no untranslated placeholders in a
  translation declared complete.
- **Translation quality.** Use professional, domain-appropriate wording; check the
  meaning of technical and scientific terms instead of translating them mechanically.
- **Font licences.** Bundle a font only when a suitable system font is not available:
  verify its identity and licence (embedding and redistribution rights), keep required
  notices, and never ship commercial fonts without the rights
  ([DEPENDENCIES](DEPENDENCIES.md) section 13). Never add fonts merely because they look
  attractive.

## 5. Alignment, spacing and colour

- **Alignment** follows the language and content: left-aligned instructional text in
  left-to-right languages, right-aligned numeric columns, centred short headings where it
  helps, direction-appropriate alignment for right-to-left scripts, script-specific
  conventions for complex scripts. Use full justification only when it improves
  readability.
- **Spacing:** consistent margins, padding, line spacing, control and icon-to-text
  spacing, panel spacing and grouping that expresses structure and hierarchy - neither
  cramped nor excessively sparse; use a consistent spacing scale where practical.
- **Colour:** a restrained palette suited to the application; consistent button,
  selection, warning and error colours; readable text with adequate contrast (e.g.
  following WCAG contrast guidance); visible focus indicators; clear disabled states;
  correct light and dark themes where supported. Avoid clashing combinations, excessive
  saturation, decorative gradients and random accents. Keep established brand colours
  unless a redesign is authorized ([LOGO_AND_BRANDING](LOGO_AND_BRANDING.md) section 9).

## 6. Responsive layout and windows

- Use the framework's layout management instead of fixed positions; support small,
  intermediate, large, maximized and (where applicable) full-screen windows, high-DPI,
  multiple monitors and mixed-DPI setups.
- When a window shrinks: reflow content, keep critical controls reachable, scroll where
  needed, use toolbar overflow, collapse secondary panels, never clip or overlap.
- When it grows: give space to the main workspace, keep controls at readable sizes, keep
  panel proportions balanced, avoid huge empty areas and oversized controls. Do not scale
  every element proportionally.
- Set minimum sizes only where justified by content; remember window size and layout
  where users expect it.

## 7. Platform conventions

- **Windows:** conventional menus and shortcuts (Ctrl-based), native file dialogs,
  per-monitor DPI awareness, taskbar integration, standard window controls, UI
  Automation accessibility, system theme, correct application icons.
- **macOS:** the application menu and menu bar conventions, Command-key shortcuts,
  native window behaviour and file dialogs, light and dark appearance, Retina rendering,
  Dock behaviour, macOS accessibility APIs.
- **Linux:** desktop-environment conventions, GTK or Qt integration as appropriate, X11
  and Wayland differences, window-manager behaviour, launcher integration, font
  rendering, icon themes, shortcuts, high-DPI, native or portal file dialogs. Desktop
  environments differ; do not assume one behaves like another.

Keep identity and essential functionality consistent across platforms while allowing
platform-specific menus, window controls, dialogs, shortcuts, fonts, icons, widgets, file
pickers, notifications and settings presentation. Pixel-identical interfaces are not a
goal; platform-appropriate usability is.

## 8. Controls and workflow

- **Necessity first.** Before adding a button, toolbar icon, menu or context-menu
  action, dropdown, panel, dialog, setting or decorative element, check the existing
  interface: add it only if it is genuinely needed, and never duplicate an existing
  function in a confusing way. Legitimate alternative access (menu, toolbar, shortcut)
  stays.
- **Placement** follows purpose and frequency of use: logical groups, consistent
  alignment, predictable navigation, suitable sizes and clear labels, tooltips where
  they help, and adequate target sizes. No arbitrary placement.
- **Nontechnical users:** for specialised applications, offer proportionate help -
  short descriptions, tooltips, step-by-step workflows, sensible defaults, clear
  validation messages, expandable explanations, documentation links - without clutter.
  Do not assume specialist knowledge unless the application targets specialists;
  scientific explanations stay technically accurate.
- **Errors and feedback:** messages say what happened and what the user can do; long
  operations show progress and allow cancellation where feasible.

## 9. Accessibility

Full keyboard operation with a logical tab order and visible focus; accessible names and
roles for controls (screen readers via the framework's accessibility API); sufficient
contrast; information never conveyed by colour alone; text that scales with system
settings; no flashing content; accessible dialogs and error messages. Test with the
platform's accessibility tools where available and report what was not checked.

## 10. Lightweight and stable UI

Keep UI overhead small: low idle CPU (no unnecessary timers or polling), reasonable idle
memory, efficient rendering without redundant redraws, bounded image caches, background
workers for heavy work, prompt cleanup of resources. Do not set universal CPU or memory
limits; heavy scientific or document processing may legitimately need substantial
resources - distinguish UI overhead from the application's workload. Check startup,
resizing, menus, scrolling, text editing, panel switching, large-file loading,
background processing, error recovery and cancellation for lag and crashes. Details and
measurement rules: [PERFORMANCE](PERFORMANCE.md) sections 5, 6 and 12. Never trade
correctness or data safety for responsiveness.

## 11. Copyright and licence presentation

- **Use the actual licence.** Determine it from the repository's authoritative licensing
  information (`LICENSE`, `COPYING`, SPDX headers, package metadata). Applications may use
  AGPL-3.0, AGPL-3.0-or-later, GPL, LGPL, MIT, Apache-2.0, BSD, custom or proprietary
  licences. Never invent, substitute or alter a licence
  ([LICENSE_POLICY](documentation/LICENSE_POLICY.md)).
- **Where to show it:** normally an About dialog or page, or Help > License, with access
  to the full licence text and third-party notices
  ([THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md)). The
  presentation fits the application, its existing About page, the available space, the
  distribution method and the platform; it need not be identical across applications.
  Legally required notices must stay reachable.
- **Copyright line:** use verified information. For example `© 2026 <Owner Name>`; with a
  proprietary licence where appropriate `© 2026 <Owner Name>. All rights reserved.`; for
  open source `© 2026 <Owner Name>` plus
  `Licensed under <the actual SPDX identifier>` (e.g. `AGPL-3.0-or-later` only when that
  is the licence). Never assume every application uses the same licence, and never
  remove third-party notices.
- **Years:** start from the documented first publication (or initial copyright) year;
  after qualifying updates in later years use a range (`© 2026–2027 <Owner Name>`). Review
  years during authorized release preparation or relevant documentation changes - never
  merely because the system clock changed - and never invent historical dates.
- **Footer vs About:** prefer the About section. A footer is optional and is omitted when
  it would crowd the interface; do not repeat copyright notices throughout the UI.

## 12. Verification

- **Functional:** every visible control that the change affects works as intended -
  buttons, menus, toolbars, dropdowns, shortcuts, settings, dialogs, file operations,
  progress indicators, error messages, saving, undo/redo where applicable.
- **Multilingual:** for each supported language, check representative labels, menus,
  dialogs, input, tooltips, font rendering, wrapping, layout expansion, Unicode, and
  where applicable shaping, right-to-left behaviour and input methods. English-only tests
  never establish multilingual support.
- **Cross-platform:** test on the operating systems the application supports and the
  environment provides - fonts, themes, DPI scaling, resizing, native dialogs, icons,
  shortcuts, layout, launch and responsiveness. Never claim Windows, Linux and macOS
  support when only one was tested.
- **Visual:** inspect the actual rendered interface (screenshots or the running
  application) for typography, colour, spacing, alignment, clipping, icon quality,
  responsive behaviour, dialog proportions, footer clutter and branding consistency.
- **Regression:** re-check the workflows the change touches; keep existing functionality.

Report source inspection, unit tests, simulated or offscreen rendering, and real
interaction separately ([TESTING](TESTING.md)).

## 13. Precedence

Application requirements decide the UI architecture, supported languages and platforms,
visual identity, licence, workflows, features and performance targets; this policy
supplies quality standards for them. Project requirements may specialise this guidance
but never override safety, privacy, legal or authorization rules
([DEVELOPMENT_POLICY](../DEVELOPMENT_POLICY.md) section 1).
