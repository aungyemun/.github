# Logo, Icons and Branding

Engineering policy for an application's visual identity: logo design principles,
creating a logo when authorized, preserving and adapting existing logos,
platform-specific application icons, variants, typography and colour in logos, asset
quality, and provenance and licensing. How the identity is used inside the interface
(layout, colour system, About page) is covered by [UI_UX_DESIGN](UI_UX_DESIGN.md);
icon embedding in installers and launchers by
[INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md) section 8; README images by
[README_POLICY](documentation/README_POLICY.md).

Not every application needs a new logo. Established official branding is reused by
default.

## 1. When a new logo may be created

Create a new logo only when the user explicitly asks for one. Routine technical work on
an existing logo - resizing, format conversion, padding correction, platform icon sets -
may proceed within an authorized UI or packaging task. A substantial artistic redesign
of an existing logo requires explicit authorization. Never create logos for existing
applications on your own initiative.

## 2. Assess before designing

Establish: the application's name and function; intended users and professional context;
existing branding; the intended visual identity; target operating systems and the icon
sizes they need; light and dark theme requirements; distribution and licensing
constraints. The logo should suit the purpose, for example:

| Kind of application | Typical character |
| --- | --- |
| Scientific software | precise, credible, modern |
| Document software | clear, professional, recognisable |
| Educational software | approachable, readable, engaging |
| Database software | structured, organised, trustworthy |
| General productivity | simple, versatile, recognisable |

These are starting points, not templates: do not give every application the same
symbols or style.

## 3. Design principles

Aim for: a recognisable silhouette; clear hierarchy and balanced composition; consistent
geometry and proportions; meaningful, differentiated symbolism; appropriate colours;
legibility at small sizes; distinctiveness; simplicity and scalability.

Avoid: excessive complexity, unnecessary gradients, tiny details that vanish at icon
sizes, cluttered or distorted shapes, crowded text, generic clichés without
differentiation and unrelated decoration. More detail does not make a better logo.

## 4. Creating a logo (when authorized)

1. Review existing assets and the application.
2. Agree the visual direction (from the assessment in section 2).
3. Develop a small number of concepts and check that each represents the application.
4. Refine the chosen concept into the needed deliverables (section 7).
5. Verify at realistic icon sizes, contrast, and transparent-background behaviour
   (section 10).
6. Record brief usage rules where the project needs them (minimum size, clear space,
   colour variants).

Use the graphics tools actually available. Describe outputs truthfully: a raster image is
not an editable vector asset, and an auto-traced vector is not hand-drawn artwork. Never
copy or imitate another product's recognisable logo, and never claim that a logo is
legally unique or trademark-safe.

## 5. Preserving an existing logo

Reuse it by default and keep its recognisable identity. Prepare proper resolution
variants from the best available source; preserve the aspect ratio (no stretching);
avoid blurry upscaling of small rasters - recreate from a vector or a larger source when
needed, and say so if none exists; correct transparent padding consistently; convert
formats and create platform sizes as required.

## 6. Platform icon assets

Prepare only what the supported platforms need:

- **Windows:** `.ico` with multiple embedded sizes (e.g. 16, 24, 32, 48, 64 and 256
  pixels), embedded in the executable's resources and used for taskbar, desktop and
  Start Menu shortcuts and the installer.
- **macOS:** `.icns` or an asset catalog with the standard sizes including @2x Retina
  versions, matching the bundle identity; check the Dock appearance and light/dark
  presentation where applicable.
- **Linux:** PNG at standard sizes and/or SVG, installed into the hicolor icon theme
  (`share/icons/hicolor/<size>/apps/`) under the application ID referenced by the
  `.desktop` file; AppImage and native packages follow their own conventions.

Do not force one image format onto every operating system. Small sizes may need a
simplified glyph rather than a downscaled full logo.

## 7. Variants

Create only the variants the application will use - for example the application icon,
full logo, compact logo, wordmark, light- and dark-background versions, monochrome, and
a high-resolution raster. Keep one consistent identity across them; do not generate
unused assets.

## 8. Text in logos

Use professional, legible type with readable spacing and preserved proportions, in a
style suited to the application; avoid decorative typefaces that reduce legibility; check
font licences (section 11). If the name is too long for a small icon, use a symbol-only
variant - never force miniature text into taskbar or Dock icons.

## 9. Colour

Coordinate logo colours with the application's interface where appropriate, without
requiring every control to use them. Keep sufficient contrast, avoid unrelated accents,
and provide variants for light and dark backgrounds where needed.

## 10. Quality verification

Check dimensions, resolution, file format, aspect ratio, transparency, edge quality
(anti-aliasing, no halos), legibility at the smallest real size, appearance on light and
dark backgrounds, platform compatibility, and that every reference in code, packaging and
documentation points to the right asset. Where practical, view the icon in the running
application, the taskbar or Dock and the launcher. Do not claim visual verification when
only file generation was tested.

## 11. Provenance and licensing

Record, proportionately: who owns the artwork; whether it is original; third-party source
material; licences of fonts, icon sets and clip art used; redistribution rights and
required attribution (update third-party notices where the project keeps them,
[THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md)). Never use
copyrighted third-party artwork without permission. Never claim guaranteed uniqueness,
register trademarks or make legal ownership claims; those are the owner's decisions.
