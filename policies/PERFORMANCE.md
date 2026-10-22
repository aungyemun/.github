# Performance and Efficiency

Engineering policy: how agents design, implement, measure and validate efficient
application behaviour. Apply it when a task concerns speed, responsiveness, resource use,
large data, concurrency or scalability, or when a change plausibly affects them. Nothing
here requires an application to contain a `PERFORMANCE.md`, and nothing here sets
universal numerical limits: targets and thresholds come from the project, its users and
measured baselines.

**Core principle:** measure the relevant bottleneck, optimise in proportion to its
impact, and verify correctness after every change.

## 1. Boundaries and priority (Mandatory safeguard)

Priority when goals conflict:

1. Correctness and scientific validity.
2. Data integrity.
3. Security and privacy.
4. Reliability.
5. Performance efficiency.

Never improve performance by: disabling required validation; skipping scientific
processing steps; ignoring or swallowing errors; reducing numerical precision or changing
methods without authorization; weakening encryption or authentication; removing integrity
checks; deleting required outputs or intermediate results; disabling essential logging;
introducing unsafe concurrency; or misrepresenting benchmark conditions.

When an optimisation involves a material trade-off (memory for speed, precision,
determinism, disk use, compatibility, complexity), state it and obtain authorization when
it changes results, behaviour or requirements. Do not optimise for benchmark scores, and
do not add complexity without demonstrated benefit.

## 2. CPU

Look for: algorithmic complexity that grows badly with input size; redundant or repeated
computation and parsing; inefficient inner loops; busy-waiting and frequent polling
(prefer events, notifications or back-off); unnecessary background tasks; worker
oversubscription (more CPU-bound workers than cores, nested thread pools, BLAS/OpenMP
threads multiplied by process pools); CPU use while idle. Prefer better algorithms and data
structures over micro-optimisation, and bounded execution over open-ended loops. Never
trade numerical accuracy or functional correctness for CPU savings.

## 3. Memory

- Consider peak and idle memory, growth over time, object lifetimes, buffer sizes and
  per-worker memory (each process holds its own copy).
- Stream or process in chunks when files or results may exceed comfortable memory, rather
  than loading everything; avoid unnecessary copies (slicing, conversions, string
  concatenation in loops, duplicate data frames).
- Bound caches (size or entry limits, eviction); release large buffers, file handles and
  native resources deterministically (context managers, `finally`, disposal), which also
  avoids Windows file-lock problems.
- Test repeated operations (open/close a document many times, process many files) for
  abnormal growth; investigate leaks with a profiler rather than guessing.
- Do not impose arbitrary memory limits; derive limits from the project's target
  hardware and data sizes.

## 4. Disk and storage

- Avoid duplicate copies of large files and unnecessary reprocessing (cache results keyed
  by input content and parameters, with invalidation).
- Manage temporary files: proper location, cleanup on all exit paths, no unbounded growth.
- Buffer I/O appropriately; bound and rotate logs.
- Use atomic writes (write to a temporary file in the same directory, flush, replace) for
  files that must not be corrupted, and support safe recovery or resumption after
  interruption.
- Check free space before large writes or downloads when practical and fail clearly.
- Never delete user files, scientific data or needed intermediate results to save space
  without authorization ([PRIVACY](PRIVACY.md) section 11).

## 5. Startup and idle behaviour

Avoid unnecessary work at startup; defer optional initialisation (plugins, large models,
indexes) until needed; avoid repeated dependency or resource discovery; minimise idle CPU
and wake-ups; prevent uncontrolled background activity; release idle resources when safe.
Measure startup in representative environments, including packaged builds and cold
starts. Do not invent universal startup-time targets.

## 6. GUI responsiveness

Design and visual standards are in [UI_UX_DESIGN](UI_UX_DESIGN.md); this section covers
responsiveness only.

- Never run expensive work (file I/O, parsing, rendering of large documents, network,
  database queries, analysis) on the UI thread; use the toolkit's worker or async
  mechanisms and update the UI only from the UI thread.
- Provide meaningful progress for long operations and cancellation where feasible;
  cancellation must leave data consistent.
- Throttle or coalesce frequent UI updates and redraws; virtualise large lists and
  tables; render large images and documents progressively or at display resolution.
- Bound image, thumbnail and document caches.
- Consider interaction latency, startup, document loading, rendering and large-dataset
  operations. Claim responsiveness improvements only with evidence (timings, profiler
  output, or observed behaviour in the running application).

## 7. Concurrency

- Size thread and process pools to the workload and available hardware; never create
  unbounded workers or one thread per item.
- Use I/O concurrency (async or threads) for waiting on I/O, processes for CPU-bound work
  where the runtime requires it; do not parallelise merely because work is heavy -
  measure the overhead and the gain.
- Guard shared state; avoid lock-order deadlocks; propagate cancellation; join or
  terminate workers and child processes on all exit paths.
- Where results must be reproducible, keep execution deterministic (stable ordering of
  outputs, controlled seeds per worker, deterministic reductions) or document the
  non-determinism and its effect.

## 8. Network performance

Use explicit timeouts; bounded retries with back-off (only for idempotent operations);
avoid redundant transfers (conditional requests, caching); support resumable downloads
where appropriate; limit parallel connections responsibly; handle interruptions. Preserve
integrity: verify size and checksums or signatures where available and validate the
completed file. A transfer tool exiting successfully is not proof of a complete, correct
download. Speed never takes priority over integrity. Avoid unnecessary background requests
([PRIVACY](PRIVACY.md) section 6).

## 9. Databases (including SQLite and SQLCipher)

Profile before changing schemas (query plans, e.g. `EXPLAIN QUERY PLAN`); add indexes
justified by measured queries; avoid unnecessary full-table scans and N+1 query patterns;
batch writes inside transactions; reuse connections appropriately; choose journal modes
(e.g. WAL) deliberately; prevent unsafe concurrent writes. Never weaken encryption,
key-derivation settings, transaction safety or integrity to gain speed. Migrations
preserve existing data and are tested on copies of real-version databases.

## 10. Large files and scientific data

Applies to FASTQ, BAM/CRAM, reference genomes, expression matrices, microscopy and other
large images, scientific databases, PDF collections and similar data.

- Stream, chunk or memory-map; use indexed access (e.g. BAM/CRAM indexes, chunked HDF5,
  Zarr, OME-TIFF tiles) instead of full loads; use established scientific libraries
  rather than hand-written parsers.
- Control RAM and disk use (intermediate files, compression levels, temporary
  directories on adequate storage).
- Handle errors explicitly; validate inputs and outputs (checksums, record counts,
  format validation); support recovery or restart of long runs where applicable.
- Record tool versions, parameters, seeds and reference versions so faster runs remain
  reproducible ([REPRODUCIBILITY](REPRODUCIBILITY.md)).
- Never speed up processing by silently changing methodology, filtering, precision,
  subsampling or parameters. Any such change is a scientific change requiring explicit
  agreement.

## 11. Cross-platform performance

Filesystem semantics and speed, file locking, antivirus scanning, path handling, process
creation cost (spawn vs fork), threading, memory allocation, CPU architecture (x86-64,
ARM64), GPU availability and drivers, runtime dependencies and packaging differ between
Windows, Linux and macOS. Do not generalise measurements to untested platforms; name the
platform and hardware for every reported result.

## 12. Benchmarking method

Before optimising: identify the concern; choose meaningful metrics; establish a baseline
with representative data; record hardware, OS, runtime and dependency versions, and
relevant settings (power mode, background load, warm or cold caches).

While optimising: make focused changes; measure the affected operation; compare
equivalent workloads; check resource use, not only time; verify correctness.

After optimising: repeat measurements enough to judge variability; compare with the
baseline; look for regressions elsewhere; state uncertainty; report actual results.

Relevant metrics may include startup time, operation latency, UI interaction latency,
CPU utilisation, peak and idle memory, disk and network throughput, query latency,
processing time and recovery time.

Never fabricate measurements. Never present estimates, expectations or results from a
different environment as measured results.

## 13. Regression prevention

- Use project-specific baselines and thresholds where the project defines them; do not
  introduce universal numerical limits.
- When practical, compare before and after: runtime, memory, startup latency, UI
  latency, disk usage, throughput **and output correctness**.
- Account for measurement noise (repeat runs, compare distributions or medians); do not
  treat small differences within noise as confirmed regressions or improvements.
- Keep performance tests out of the default fast test path when they are slow or
  environment-sensitive; mark them and document how to run them.
- An optimisation has not succeeded if correctness tests fail.
