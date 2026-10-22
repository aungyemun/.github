# Scientific Methods Documentation Policy

Applies only to scientific and research software.

Require accurate terminology and nomenclature; clear descriptions of input data;
reproducible procedures with software, reference data and database versions; the
statistical methods actually implemented, with assumptions and multiple-testing handling;
quality-control criteria; provenance; all result-affecting parameters; limitations; and
transparent interpretation.

Never fabricate results, statistics, figures or citations. Never silently change method
descriptions or conclusions; if implementation and documentation disagree, report it and
change either only with the maintainer's agreement. Unit-test success is not scientific
validation. Never include identifiable participant or patient data.

Describe, as relevant, the implemented stages, algorithms, software versions, parameters,
reference assemblies and annotation releases (recorded separately), input data and
accessions, quality control, statistical models, output interpretation, references and
known limitations - only for methods the code actually runs. Use the project's citation
style (numbered Vancouver style for biomedical software when none exists) and keep one
authoritative reference list, in `METHODS.md` or an optional `docs/SCIENTIFIC_REFERENCES.md`.
Provenance and citation verification: [SCIENTIFIC_PROVENANCE](../SCIENTIFIC_PROVENANCE.md).
