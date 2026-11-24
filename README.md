# Dabia2013 benchmark family (TDVRPTW and TDVRP, TD-IGP-Solomon)

Benchmark family repository of [MAMUT-routing](https://github.com/ANR-MAMUT/MAMUT-routing), mounted there as `benchmarks/Dabia2013`. It holds the 2 problem variants of the family, TDVRPTW and its time-window-free TDVRP variant.

Canonical MAMUT-routing curation of the classic Duration-Minimization TDVRPTW benchmark introduced by Dabia et al. (2013). It combines Solomon 1987 instances (25/50/100 customers) with IGP time-dependent travel times (Ichoua et al. 2003), as distributed with the open-source solver of Lera-Romero et al. (2020).

## The TDVRP variant

The TDVRP variant is the Duration-Minimization TDVRP variant of the same benchmark. It holds the exact same instances with time windows removed (service times, demands, capacity, fleet size and time-dependent travel times unchanged, and the depot departure time remains a decision variable). Its `.vrp.json` files simply omit `time_windows`, and the 2 variants reference the same ATF sidecars under `sidecars/` (same sha256). The format, conventions, provenance and licensing described below apply to both variants.

Every TDVRP instance carries a `Duration` BKS. The published Dabia2013 solutions are TDVRPTW solutions, so no historic TW-free solutions existed for this variant.

## Layout

- `TDVRPTW/n=<customers>/` and `TDVRP/n=<customers>/` hold the instances (`<Name>.vrp.json`) and their best-known solutions (`<Name>.bks.Duration.json`) of each variant, for example `TDVRPTW/n=25/C101.vrp.json` and `TDVRP/n=25/C101.bks.Duration.json`.
- `sidecars/n=<customers>/` holds the ATF sidecars, stored once and shared by the 2 variants, for example `sidecars/n=25/C101.atf.json` and `sidecars/n=50/C101.atf.json.gz`. Each instance references its sidecar by a path relative to the repository root, with the SHA-256 of the sidecar content.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- `history/` holds the commit map of the former location of this family (see the history section below).

## Format

Each instance has 3 artifacts:

- `<Name>.vrp.json`: raw instance data in the MAMUT shape, with depot index 0 (no duplicated end depot, contrary to historic distributions), `num_customers` counting customers only, mandatory `coordinates`, `horizon`, and a `td` block referencing the ATF sidecar with its storage-independent sha256.
- `<Name>.atf.json` (plain, n < 50) or `<Name>.atf.json.gz` (n ≥ 50): the canonical ground truth, one arrival-time NDCPWLF per arc of the complete graph, exact consolidation of the raw IGP data. Breakpoints sit exactly where departure or arrival crosses a speed-zone boundary, and the last zone's speed extends beyond the horizon so every ATF is total on `[0, T]`.
- `<Name>.bks.Duration.json`: best known solution under the `Duration` objective. Edition `r1` uses `td-binary64-exact-v1`. The checker evaluates the materialized binary64 arc data with exact rational arithmetic and records each route's minimum duration and earliest departure attaining it. The certified solution cost is the fraction in `metadata.standard.authoritative_cost`. The top-level `cost` is its binary64 display. The TDVRPTW BKS were seeded from the published Lera-Romero et al. (2020) solutions, all re-validated during the original import (146/146 feasible, several of them with arrivals exactly on time-window deadlines, which fragile recomputation pipelines misreport as infeasible).

Population pipeline: `populate_td_dabia2013` v1, one-shot curation tooling maintained outside this repository (links to the migration scripts will be published with the benchmark paper). It cross-validates every consolidated ATF against direct Ichoua evaluation and reloads every written artifact through the full validation path. The `generator` block of every artifact records the pipeline name and version.

## Library compatibility

This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the archive of the former main repository, [MAMUT-routing-archive-2026-11](https://github.com/ANR-MAMUT/MAMUT-routing-archive-2026-11).

## Provenance and licensing

Instance definitions derive from Solomon (1987) and the TD extension of Dabia et al. (2013) / Ichoua et al. (2003). The raw JSON sources are those shipped with Lera-Romero et al. (2020), https://doi.org/10.1002/net.21937. MAMUT-routing-authored curation artifacts are distributed under the MIT license (see [LICENSE](LICENSE)). This notice does not relicense the underlying third-party benchmark definitions.

## Citation

The citation metadata of this repository, with its authors and their ORCID identifiers, is recorded in [CITATION.cff](CITATION.cff). This family is a curation of a third-party benchmark, so also cite the original benchmark: Solomon (1987) for the instance definitions, Dabia et al. (2013) for the time-dependent extension, Ichoua et al. (2003) for the travel-time model, and Lera-Romero et al. (2020), https://doi.org/10.1002/net.21937, for the distributed raw sources. Record the exact Git commit used in experiments.

## History of this repository

Until November 2026 the 2 variants were tracked directly in the main MAMUT-routing repository, at `benchmarks/TDVRPTW/Dabia2013` and `benchmarks/TDVRP/Dabia2013`. Their history was extracted from the history of the main repository and rewritten under the directory of each problem type (`TDVRPTW/` or `TDVRP/`). Only the commits that touched these 2 directories are kept, each with a new identifier. `history/commit-map-MAMUT-routing.tsv` maps the former commit identifiers of the main repository to the new ones, for the kept commits only. It is tab-separated, with the former commit identifier and the new one on each line, after a header line. See [CHANGELOG.md](CHANGELOG.md) for the reorganisation.
