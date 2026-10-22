# Third-Party Notices Policy

How to prepare an application's `THIRD_PARTY_NOTICES.md` (or the project's equivalent:
`NOTICE`, a licences folder, an About > Third-party licences page). It is separate from
the application's own licence ([LICENSE_POLICY](LICENSE_POLICY.md)). Choosing and
managing dependencies, and their licence compatibility checks, are engineering rules in
[DEPENDENCIES](../DEPENDENCIES.md) sections 1 and 13; this policy covers identifying what
is distributed and documenting it correctly. This is engineering guidance, not legal
advice.

## 1. Identify what is actually distributed

Derive the list from evidence - lockfiles, build manifests, packaging specs and the
contents of the built package - not from memory. Relevant components include runtime
libraries, UI frameworks and their plugins, frozen-executable runtimes, external
executables, scientific software, OCR engines and language data, fonts, icons, images,
models, reference datasets, bundled system libraries and redistributed transitive
dependencies.

Distinguish development-only dependencies (linters, test frameworks, build tools that are
not shipped), which normally need no end-user notice, from components shipped to users.
Record the basis for including or omitting each borderline component.

## 2. Inventory per component

Verify from the component's own source (package metadata, LICENSE/COPYING/NOTICE files,
SPDX identifiers, upstream repository):
1. official name;
2. exact version included;
3. upstream source or homepage;
4. copyright holders, preserved exactly as the upstream states them;
5. licence (SPDX identifier where one exists);
6. how the application uses it;
7. whether it is redistributed, and in what form (source, binary, statically or
   dynamically linked, modified or unmodified);
8. required attribution;
9. required licence texts;
10. other obligations (Apache-2.0 `NOTICE` files, LGPL relinking and source-offer
    conditions, GPL/AGPL source availability, font reserved-name rules, data-licence
    attribution or share-alike terms).

Never invent or guess versions, licences or copyright holders. If something cannot be
determined, record "not verified" and tell the maintainer; never ship an assumption.

## 3. Licence obligations

Licences are not interchangeable. MIT, BSD, Apache-2.0, GPL, LGPL, AGPL, proprietary,
custom, font (e.g. SIL OFL) and data licences carry different notice, source-availability
and compatibility obligations. Identify the obligations that follow from how the
component is distributed; never change the application's own licence to resolve a
conflict without the user's authorization; raise unresolved material conflicts with the
user; and state uncertain conclusions as uncertain.

## 4. Contents of the notices document

For each relevant component: name, version, copyright line(s), licence identification,
official source, use and distribution status, required acknowledgements and notices, and
the required licence text - inline or as accompanying licence files referenced from the
document. Use a clear, maintainable structure (for example a table followed by grouped
licence texts, one copy of each identical licence text with the components it covers).
Never remove legally required text to shorten the document.

## 5. Fonts, icons, images and branding assets

Verify licences of bundled fonts, icon libraries, illustrations, logo components, images
and UI themes. Material found on the internet is not automatically redistributable; never
use third-party branding assets without the right to do so; keep required attribution
([LOGO_AND_BRANDING](../LOGO_AND_BRANDING.md) section 11).

## 6. Scientific tools and reference resources

Review licences and provenance of bioinformatics executables, statistical packages,
genome references, annotation databases, scientific datasets, dictionaries, ontologies
and specialised analysis libraries. Citation requests and software licence obligations
are distinct: include required citations or acknowledgements where the upstream asks for
them, in addition to licence notices. Free of charge does not mean unrestricted
(academic-only or non-commercial terms are common). For each redistributed or legally
relevant scientific component, cover identity, version, copyright holder, licence,
official source, redistribution requirements, required notices and licence text; a
citation never substitutes for a licence obligation. Keep methodology descriptions out of
the notices: point to `METHODS.md` or `docs/SCIENTIFIC_REFERENCES.md` instead
([SCIENTIFIC_PROVENANCE](../SCIENTIFIC_PROVENANCE.md)).

## 7. Keeping notices current

Whenever a relevant component is added, removed, updated, replaced, bundled differently
or modified, review the notices in the same change, and confirm they match what is
actually distributed. Do not delete notices for components that older, still-distributed
versions contain without checking the distribution and legal requirements.

## 8. Notices in installers and packages

Decide how licence information reaches users of each distribution: installed
documentation, a legal-notices directory, About > Third-party licences, files inside the
archive, or platform-appropriate package locations (e.g. `/usr/share/doc/<package>/` for
DEB packages, the application bundle's resources on macOS). Layouts may differ by
platform; obligations must be met on each. Do not ship unrelated development
documentation in installer packages.

## 9. Verification

Compare the notices with the built package contents and the lockfile; check every
licence text is present and complete; check versions; report components whose licence
or version could not be verified.
