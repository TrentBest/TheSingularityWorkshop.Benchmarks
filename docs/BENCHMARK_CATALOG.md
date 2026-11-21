# Benchmark catalog and evidence contract

This is the cross-repository registry for measurable performance behavior in The Singularity Workshop. **Registered** means a harness/workload is known; it does not imply complete coverage, a current baseline, or an active regression gate.

## 1. Permanent identities
Use stable semantic IDs, not method names or parameter values. Recommended prefixes: FSM (runtime), COS (composition/orchestration), SER (serialization), GUI, REN (Renderer), VIS (Visualization), DW (DataWarehouse), AI, and SYS (cross-domain). Never reuse an ID for a different operation. Preserve IDs across implementation refactors when semantics remain equivalent. Retire explicitly with a reason; retain history. Parameter dimensions belong in result metadata.

## 2. Required run provenance
Record:
- Benchmark ID, semantic description, timed-region definition, workload parameters and fixture/input distribution.
- Harness repository + exact commit; catalog revision; source repository + exact commit, or package ID/version.
- Explicit lane: `published-package`, `development-source`, or another named build mode.
- Runner/BenchmarkDotNet version, SDK, target framework, runtime, configuration and command/filter.
- OS, architecture, CPU model, core/thread count and relevant virtualization/power constraints when available.
- Mean and distribution statistics (standard deviation and percentiles when available), throughput, allocated bytes/op and GC counts where applicable.
- Immutable raw reports, logs and artifact links.

Missing essential provenance is **unverified historical data**, not a baseline. Never infer missing hardware, source SHA, package version or runtime.

## 3. Measurement classes
**Operation:** narrow timed operation; keep setup, fixture construction and correctness assertions outside timing unless they are the subject.
**Component:** realistic subsystem path; state included boundaries and host/runtime overhead.
**Composite:** end-to-end consumer path across components; keep it visible beside internal diagnostics.
**Stress/capacity:** controlled scale/workload sweep; record latency, allocation, throughput and correctness boundaries. Do not present stress numbers as microbenchmarks.

## 4. Comparison and regression
Compare only equivalent semantics and compatible lanes. Published-package and source-project results remain separate. Append evidence; never overwrite history. Begin report-only, characterize noise, then stage gates: evidence validity, correctness, calibrated performance regression. Missing artifacts, mismatched SHAs, failed correctness, empty reports or incompatible environments are inconclusive (fail closed). Explain slowdowns, including security/validation/correctness costs; do not remove safeguards to improve scores.

## 5. Initial registry
| Harness | ID family | Known coverage | Qualification / gaps |
|---|---|---|---|
| [FSM_API_Benchmark](https://github.com/TrentBest/FSM_API_Benchmark) | FSM-* | Single-FSM and processing-group update entry points; broader suite described in open work | Historical diagnostics/timings need provenance verification; automation/baselines not verified |
| [FSM_COS_Benchmarks](https://github.com/TrentBest/FSM_COS_Benchmarks) | COS-* | `master` contains a SHA-256 primitive; `development` defines COS-CAT, COS-MAN, COS-CMP, COS-DEP, COS-ERR, COS-LOAD, and COS-RT workloads | Run #42167035105 passed 101 cases across 15 CSV reports (harness `ffe495925401ea19e8d9e0fc7d456656402f5ec5`, source `207b417a7958134bbc138374a5d077b707e1faf3`); exploratory ShortRun on virtualized Windows 11/Hyper-V, not a stable baseline |
| [FSM_Serialization_Benchmarks](https://github.com/TrentBest/FSM_Serialization_Benchmarks) | SER-* | Stream/buffer strategies, fixed-width vs ULEB128 encode/decode; Experience Brotli work in an open draft | Keep package/source lanes separate; definition codecs, integer round-trip, property tests, adapters and cross-repo dispatch remain incomplete |
| This repository | SYS-* | Cross-repository catalog/policy | No composite execution or aggregated result history claimed |

GUI, Renderer, Visualization, DataWarehouse and AI are candidates, not implemented benchmark domains, until executable workloads are inspected and registered.

## 6. Domain priorities
### FSM_API
Measure string- and integer-backed paths without presuming a winner: update/step, transitions, state/definition cardinality, live-instance scaling, processing-group lookup, scheduler rates, lifecycle, timers, errors and mutation. Sweep meaningful scales; record latency distributions, throughput, allocations/GC, crossover and failure boundaries. Keep the public consumer path beside internal diagnostics.

### FSM_COS
Measure manifest parsing/validation, dependency resolution, microbundle graph composition, unresolved dependencies, repeated composition, caching and execution handoff. Define graph shape/size. A hash microbenchmark is useful only as a separately labeled primitive.

### FSM_COS evidence note
The `development` harness is materially ahead of the `master` branch: it includes stable-ID catalog lookup, manifest construction/staged validation, empty and multi-root composition, dependency chains/fan-out/shared dependencies, duplicate requests, missing/conflicting-version failures, stateful load callbacks, and RuntimeAssembly lookup comparisons. The development README/catalog documents a successful 101-case run (15 CSV reports) in [workflow run #42167035105](https://github.com/TrentBest/FSM_COS_Benchmarks/actions/runs/42167035105), with raw evidence retained in artifact `fsm-cos-benchmark-42167035105-1-ffe495925401ea19e8d9e0fc7d456656402f5ec5`.

That run used BenchmarkDotNet 0.15.2 and a three-iteration ShortRunJob on a virtualized Windows 11 / Hyper-V runner. It demonstrates that the configured workloads executed and reports were retained; it does not establish publication-quality baselines or calibrated regression thresholds. The measured harness commit was `ffe495925401ea19e8d9e0fc7d456656402f5ec5` and the FSM_COS source commit was `207b417a7958134bbc138374a5d077b707e1faf3`. The current `master` branch still has the separate SHA-256 primitive benchmark; do not confuse branch states or compare the primitive to composition workloads.

### Serialization
Separate published NuGet and development-source lanes. Measure encode, decode and round-trip independently; include FSM definition/experience semantics, buffer ownership/copying/pooling, compression ratio and correctness at boundaries. Report bytes saved alongside CPU and allocation costs.

### GUI, Renderer, Visualization
Measure isolated primitives and consumer-visible composition. Distinguish textual/AI-side Renderer work from pixel/world Visualization. Honor MicroBundle consumption; measure composition, initialization, steady-state update and end-to-end output separately.

### DataWarehouse and AI
For storage, separate lookup, write, iteration, serialization, cache and memory pressure. For AI, identify model/runtime/hardware and separate tokenization, orchestration, inference and end-to-end agent costs.

## 7. Automation status
This is an inventory/policy document, not a running regression service. Cross-repository dispatch, artifact ingestion, trend visualization, baseline calibration and automated interpretation remain future work until workflows and successful artifacts prove otherwise.


Detailed measured results for this run are recorded in [the FSM_COS exploratory performance snapshot](evidence/FSM_COS_RUN_42167035105.md). Highlights include flat catalog lookup (~4.4 ns across 1–1,000 entries), RuntimeAssembly ID lookup scaling to ~4.14 μs at 1,000 entries, and allocation-heavy composition workloads. The snapshot preserves mean, standard deviation, allocation/GC data, derived throughput, provenance, and explicit limitations; it is not a calibrated baseline.
