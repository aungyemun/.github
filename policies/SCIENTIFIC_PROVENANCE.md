# Scientific Provenance and References

Engineering policy for scientific and research software: identifying the methods
actually implemented, verified publication citations, scientific software inventories,
reference genome and annotation provenance, official sources, traceable analysis
records, and how scientific changes are handled.

**Applies only** to scientific or research software (indicators such as analysis
pipelines, scientific libraries, `METHODS.md`, `CITATION.cff`, the scientific-software
profile) or when the maintainer says so. Never impose scientific citation or provenance
requirements on unrelated applications.

Responsibilities are separate; use cross-references rather than copies:

| Topic | Policy |
| --- | --- |
| Dependency identity, sources, versions, security, licences, redistribution | [DEPENDENCIES](DEPENDENCIES.md) |
| Rebuilding environments and analyses; numerical changes | [REPRODUCIBILITY](REPRODUCIBILITY.md) |
| Writing methods documentation | [METHODS_POLICY](documentation/METHODS_POLICY.md) |
| Writing third-party notices (licences, copyright, required notices) | [THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md) |
| Methods, references, resource provenance, version identification (this policy) | SCIENTIFIC_PROVENANCE |

## 1. Methods actually implemented

Identify the methods the code really implements - for example sequence alignment, read
counting, quality control, normalisation, differential expression, statistical testing,
image analysis, biological annotation, database processing or other algorithms - by
reading the code and configuration. For each, record: name, scientific purpose, original
publication, later methodological references where the implementation follows them,
how it is implemented (tool, function, or own code), important parameters and defaults,
and known limitations. A planned or optional method that the code does not run is not
documented as implemented.

## 2. Citations

- Use the project's citation style. If there is none, recommend numbered Vancouver-style
  references for biomedical software.
- Verify every element against an authoritative record (publisher page, DOI metadata via
  doi.org or Crossref, PubMed, or another established scholarly database): authors,
  title, journal, year, volume and issue, pages or article number, DOI - and that the
  publication describes the method actually used.
- Cite the work that defines or describes the method or tool used, not merely a paper
  that mentions a related method.
- Follow citation requests of upstream tools and databases (e.g. "please cite" sections,
  `CITATION.cff`, database terms of use).
- Never fabricate or "complete" a citation from memory. If an element cannot be verified,
  mark it unresolved.

## 3. Scientific software inventory

For each scientific tool the application uses, record: official name; developer or
project; exact version; official website; official source repository; methodological
role; the relevant publication and DOI; licence; redistribution status; relevant
parameters; supported and tested platforms where they matter. Classify each as bundled,
external (installed by the user), optional, build-only, or a runtime requirement. Never
invent developers, versions or licences; record "not verified" instead.

## 4. Reference genome and annotation provenance

For genome-based analyses record separately:
- organism (scientific name, e.g. *Mus musculus*) and, where relevant, the sample strain;
- reference assembly name and accession with version (e.g. GRCm39 and its GenBank or
  RefSeq assembly accession);
- reference provider (Ensembl, NCBI RefSeq, GENCODE, UCSC);
- exact sequence files (names, e.g. primary assembly vs toplevel, soft-masked or not);
- annotation source and release, and the exact GTF/GFF file;
- retrieval URL and date, and checksums;
- index-generation tool, version and parameters (e.g. aligner index options).

Never describe a reference only as "the mouse genome" or "the human genome" when
results depend on it. Sample strain and reference assembly are different facts (the
mouse reference assembly is derived from C57BL/6J; samples may be another strain).

### Ensembl specifics

Distinguish the Ensembl project, the Ensembl release number, the genome assembly, the
genome sequence files, the gene annotation files, stable gene identifiers and their
version suffixes (the `.N` after an `ENSMUSG` identifier). A release number does not
identify an assembly and an assembly does not identify a release; files in one analysis
may come from different releases - check each file. Record whether identifiers are used with or without version
suffixes. Never invent missing release information: report it as unverified.

The same principles apply to every organism and provider; do not hard-code
organism-specific assumptions into unrelated projects.

## 5. Official sources and links

Prefer authoritative sources such as Ensembl, NCBI GEO, NCBI SRA, RefSeq, GENCODE,
UniProt, Bioconductor, CRAN, official upstream repositories and publisher websites.
Record the organisation, resource name, official homepage, a version-specific download
URL where one exists (preferred over a generic homepage), stable accessions or
identifiers, and the retrieval date where useful. Check links where practical. Never
fabricate websites, URLs or accessions.

## 6. Analysis records

Coordinate with [REPRODUCIBILITY](REPRODUCIBILITY.md): results should be traceable to
the application source revision, software environment, external executable and
dependency versions, reference resource versions, input accessions with checksums,
important parameters, output provenance and validation status. Provenance makes an
analysis reconstructable and auditable; it does not guarantee bit-identical results in
every environment, and documentation must not imply that.

## 7. Scientific changes

When a change touches algorithms, statistical procedures, reference genomes, annotation
releases, scientific software versions, significant parameters, normalisation or
quality-control rules:
1. review the affected scientific documentation (methods, references, provenance);
2. determine whether outputs change, and by how much, using scientific regression tests
   with justified tolerances;
3. update citations and provenance records;
4. describe the change as a scientific change in the changelog - never as routine
   dependency maintenance;
5. obtain the maintainer's agreement before changes that alter results.

## 8. Application documents and their roles

| Document (only when the project needs it) | Content |
| --- | --- |
| `LICENSE` | the application's own licence |
| `THIRD_PARTY_NOTICES.md` | third-party copyright, licences, required notices; a short pointer to scientific references |
| `METHODS.md` | implemented methodology, parameters, references, limitations |
| `docs/SCIENTIFIC_REFERENCES.md` | optional single bibliography when a separate list materially improves organisation |
| reproducibility documentation | environment and analysis reconstruction |

Keep one authoritative reference list with consistent numbering; if `METHODS.md` already
holds a complete, maintainable reference section, do not create a second bibliography.
Academic citations never replace licence obligations, and licence notices never replace
citations. Never create all of these documents by default.

## 9. Platform-specific distributions

Windows, Linux and macOS packages may bundle different components. Check notices and
references against what each package actually contains; place required legal notices in
each platform's appropriate location ([THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md)
section 8); shared scientific references may live in common documentation.

## 10. Verification

Check citation existence and DOI accuracy, official software sources, actual tool
versions (`--version` output, lockfiles), the actual genome assembly and annotation
release (file headers, provider metadata, checksums), reference file provenance, licence
accuracy and redistribution relevance, and that documentation matches the
implementation. Use authoritative records; mark anything unverifiable as unresolved.
