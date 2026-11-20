<!-- The Singularity Workshop moniker -->
<p align="center"><a href="https://github.com/TrentBest"><img src="https://raw.githubusercontent.com/TrentBest/FSM_COS/master/Documentation/Branding/TheSingularityWorkshop.png" alt="The Singularity Workshop" width="720" /></a></p>

# The Singularity Workshop — Benchmarks

[![Sith Policy](https://img.shields.io/badge/branches-master%20%2B%20development-blueviolet)](https://github.com/TrentBest/TheSingularityWorkshop.Benchmarks/branches)

## Purpose
This repository is the index and governance contract for performance evidence across The Singularity Workshop. Domain repositories own executable workloads, fixtures, correctness checks, and raw profiler artifacts; this repository defines stable identities, evidence requirements, interpretation rules, and cross-repository reporting. The objective is to measure the computational operating envelope of the FSM ecosystem—not merely collect attractive timing numbers.

## Architecture
- **Operation benchmarks** isolate a defined operation and timed region.
- **Component benchmarks** measure a subsystem with realistic fixtures and explicit boundaries.
- **Composite benchmarks** measure consumer-visible paths across multiple components.
- **Stress/capacity tests** establish scaling limits and failure boundaries; never silently mix them with microbenchmark results.

Renderer and Visualization are consumed as MicroBundles. Their benchmarks must measure composition and execution in that consumption model, not assume NuGet package boundaries.

## Registered benchmark domains
| Domain | Harness | Evidence status |
|---|---|---|
| FSM runtime | [FSM_API_Benchmark](https://github.com/TrentBest/FSM_API_Benchmark) | Existing operation/stress harness; historical timings require provenance verification; report-only regression posture |
| Compute operating system | [FSM_COS_Benchmarks](https://github.com/TrentBest/FSM_COS_Benchmarks) | Current master source is a SHA-256 workload, not representative FSM_COS coverage |
| Serialization | [FSM_Serialization_Benchmarks](https://github.com/TrentBest/FSM_Serialization_Benchmarks) | Published-package and development-source lanes separated; harness incomplete |
| Cross-ecosystem index | This repository | Catalog and contract established; no composite results claimed |

GUI, Renderer, Visualization, DataWarehouse, and AI are candidates for registration after their executable harnesses and ownership boundaries are verified. A proposed domain is not evidence that a benchmark exists or has run. See [the benchmark catalog](docs/BENCHMARK_CATALOG.md).

## Evidence model
Every comparable result identifies benchmark ID, source and harness commits, package/version or source lane, workload parameters, runtime, OS/CPU, configuration, and dependency versions. Preserve raw artifacts immutably. Correctness checks are independent of timed operations wherever practical. Report distributions, throughput, allocations, and GC behavior where relevant; disclose setup, copying, pooling, ownership, security, validation, and instrumentation costs.

Regression policy starts **report-only**. Gates require repeatable baselines, controlled environments, noise characterization, and reviewed thresholds. Missing or invalid evidence is inconclusive, never a performance pass.

## Contributing and safety
Add workloads in the owning domain repository, then register permanent IDs and coverage status here. Keep published-package consumer baselines separate from development-source measurements. This repository does not authorize package publishing, merging domain PRs, deployment, or branch deletion; those actions require the owner's explicit approval.
