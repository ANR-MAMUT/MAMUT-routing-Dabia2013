# Changelog: Dabia2013 benchmark family

All notable changes to the Dabia2013 family are recorded here, the most recent first. The entry "2026-11: family repository" records the creation of this repository. The sections after it hold the changelogs that the 2 variants had in the main MAMUT-routing repository. They keep every entry, fact, date, count and commit reference, with the wording edited for style and the headings moved 2 levels down.

## 2026-11-26. Improved Duration records

- Florian Rascoussier improved the total route duration for 10 TDVRPTW instances with Kayros 2.0.0.dev0. [Kayros development revision `dba5688`](https://github.com/0nyr/kayros/commit/dba56886167c854d2ae36b627128d6d419f811f4)
- Replaying those routes without time windows on the corresponding TDVRP instances retained all existing TDVRP incumbents. The native and Python Fraction evaluators independently checked the exact durations and the earliest minimizing departure for each route under the edition `r1` profile, `td-binary64-exact-v1`.

## 2026-11-25: optimality claims withdrawn from edition r1

- The 154 solver-reported optimality claims (`metadata.optimality`) of this family's best-known solutions are withdrawn, 40 TDVRP and 114 TDVRPTW. They targeted the historical `td-fold/1` evaluation of earlier Kayros versions, not the `td-binary64-exact-v1` profile of edition `r1`. New claims will be derived with Kayros 2.0 under `td-binary64-exact-v1`.
- The 154 content-addressed evidence files under `.mamut/history/` that only these claims referenced are removed. The archive of the original pre-migration best-known solutions, indexed by `.mamut/archives/r1-original-bks.json`, is unchanged.
- No route, cost, route witness, manifest or instance file changed.

## 2026-11: family repository

- The TDVRPTW and TDVRP variants of Dabia2013, tracked until now directly in the main MAMUT-routing repository, form 1 repository, `MAMUT-routing-Dabia2013`, mounted in MAMUT-routing at `benchmarks/Dabia2013`. Their history is extracted from the history of the main repository, each variant under the directory of its problem type. `history/` holds the map from the former to the new commit identifiers.
- Instances and best-known solutions keep their former path after the problem type, for example `TDVRP/n=25/C101.vrp.json`.
- The ATF sidecars move to `sidecars/`, stored once for the 2 variants: 168 sidecars kept, 168 duplicate copies removed. In 336 instances the sidecar path field now holds a path relative to the repository root, for example `sidecars/n=25/C101.atf.json`. No sidecar content changed, and the sidecar SHA-256 recorded in each instance is unchanged.
- The SHA-256 of the 336 instance files changed because of the rewritten path field. No instance identifier changed, and no best-known solution changed.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the archive of the former main repository, `MAMUT-routing-archive-2026-11`.
- The README, LICENSE and CHANGELOG files of the 2 variants are replaced by 1 file each at the root, and CITATION.cff and NOTICE files are added. LICENSE now contains the MIT License text that it refers to.

## Former TDVRPTW directory of the main repository

### Changelog of the Dabia2013 TDVRPTW BKS

All notable changes to the curated `Dabia2013` TDVRPTW best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 168 BKS were re-priced with `mamut-routing bks reprice-td`: 143 files were rewritten, 42 of them because the cost moved (by at most 5.7e-16 relative, float rounding only), the other 101 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date). 26 optimality-stamped BKS moved (by at most 1.5e-11): their `proven_optimum` now equals the re-priced cost, and a `note` records that the proof was obtained under `td-fold/1`, that `dual_bound` is the prover's value under that arithmetic, and that re-certification under `td-fold/2` is pending.

#### 2026-08-06

**114 of the family's 115 optimality certificates re-derived from scratch and re-stamped** under the 4-solve agreement protocol (cold and warm starts crossed with the 2 labeling modes, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns) on Grid'5000, on the repaired kayros 1.5.1 build. The campaign was motivated by the 2026-08-05 withdrawal of 1 certificate in the Vu2020 TDVRPTW family and the subsequent finding that the certifying builds carried since-repaired pricing defects (documented in the kayros 1.5.1 release notes and CHANGELOG). Every re-issued stamp certifies bit-exactly the previously stored value. No solution data changed, and the stamps' provenance now cites this campaign and build.

**The RC107 (n=50) certificate is WITHDRAWN.** The campaign produced a 4-way disagreement on it: 1 solve closed at 10628.594091004356 while the other 3 closed at the stored 10614.011179781175, so the optimality claim cannot be re-derived under the agreement protocol and the 2026-07-10 stamp does not stand. The best-known solution itself is unchanged and remains checker-valid at 10614.011179781175.

#### 2026-07-13

**7 further n=100 BKS stamped proven optimal, 2 of them checker-valid strict improvements** by the long-limit continuation of the weekend re-certification (2026-07-11/12/13, Grid'5000; time limit 7200 s at n=100): R104, R107, R108, R110, RC102, RC103, RC107, of which R104 (by 34.37 duration units) and R110 (by 16.54) strictly improve the stored references inherited from the exact literature. Protocol unchanged: 4 independent exact solves (cold and warm starts crossed with the 2 labeling modes) agreeing on 1 checker-exact value, an audited exact-pricing phase in every run, and no checker-infeasible priced column. The exact value-jump arm is folded direction-aware where present. This family now carries 115 certified TDVRPTW BKS, including 22 at n=100 (all under the audited protocol).

#### 2026-07-12

**12 further BKS stamped proven optimal, 1 stale stamp retracted.** The weekend top-up of the exact re-certification campaign (2026-07-11/12, Grid'5000; time limits 1800 s, raised to 7200 s at n=100) certified C204, RC204, RC208 (n=25); C104, C205, C206, R101, R105, RC201 (n=50); and R101, R106, RC105 (n=100), the first n=100 certificates of this top-up. Protocol unchanged from the 2026-07-10/11 campaign: 4 independent exact solves (cold and warm starts crossed with the 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Separately, the last stamp surviving from the superseded 2026-07-07 campaign (C202 n=50, pre-audit prover metadata) is retracted: the pricing-ladder audit voided its trust basis and the audited campaigns leave the instance open, so its value returns to ordinary best-known status. This family now carries 108 certified TDVRPTW BKS, all from the audited protocol.

#### 2026-07-11

**96 BKS stamped proven optimal** (`metadata.optimality`), including 15 at n=100, under a full re-certification (the earlier certificates' producer carried a pricing-ladder termination defect, so stamps were regenerated from scratch under a stronger protocol). Each stamp certifies 4 independent exact solves (cold and warm starts x 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp.

#### 2026-07-10

2 BKS improved at n=100 (RC101: 24778.294 -> 24762.734; RC106: 21238.247 -> 21233.331) by exact solves recorded as ordinary best-known solutions (no optimality stamps), from a re-certification campaign running the exact solver 4 times per instance (cold/warm starts x 2 labeling modes) with 4-way value agreement, an audited exact-pricing phase, and canonical-checker re-validation.

#### 2026-07-08

21 of 168 BKS improved (mean -2.80%, largest single improvement -4.67%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-07

1 BKS improved by an exact solve and **proven optimal**: RC106 n=50 (11760.356005 → 11756.555984), by kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50. The same campaign certified 87 of the other stored Dabia TDVRPTW n=25/50 BKS optimal as stored (54 of 56 at n=25).

Structured optimality metadata: all 88 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object with the prover, certificate wording, proven optimum, dual bound and wall time (schema `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0).

#### 2026-07-06

18 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-05

6 BKS improved during the initial large-scale seeding sweep across all 4 TD families (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-02

Initial population, reaching full 168/168 BKS coverage:

- 146 BKS seeded from the solutions published with Lera-Romero et al. (2020), all re-validated and re-priced by the canonical checker.
- 22 BKS added and 16 improved from a former heuristic BKS store (2024–2026), re-validated.

## Former TDVRP directory of the main repository

### Changelog of the Dabia2013 TDVRP BKS

All notable changes to the curated `Dabia2013` TDVRP best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 168 BKS were re-priced with `mamut-routing bks reprice-td`: 138 files were rewritten, 71 of them because the cost moved (by at most 6.6e-16 relative, float rounding only), the other 67 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date). 21 optimality-stamped BKS moved (by at most 7.3e-12): their `proven_optimum` now equals the re-priced cost, and a `note` records that the proof was obtained under `td-fold/1`, that `dual_bound` is the prover's value under that arithmetic, and that re-certification under `td-fold/2` is pending.

#### 2026-08-06

**All 40 optimality certificates re-derived from scratch and re-stamped** under the 4-solve agreement protocol (cold and warm starts crossed with the 2 labeling modes, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns) on Grid'5000, on the repaired kayros 1.5.1 build. The campaign was motivated by the 2026-08-05 withdrawal of 1 certificate in the Vu2020 TDVRPTW family and the subsequent finding that the certifying builds carried since-repaired pricing defects (documented in the kayros 1.5.1 release notes and CHANGELOG). Every re-issued stamp certifies bit-exactly the previously stored value. No solution data changed, and the stamps' provenance now cites this campaign and build.

#### 2026-07-12

**8 stale optimality stamps retracted** (RC101 to RC108, n=50): the last stamps surviving from the superseded 2026-07-07 campaign carried pre-audit prover metadata, and the pricing-ladder audit voided their trust basis. The audited 2026-07-10/11 campaign and its 2026-07-11/12 weekend top-up leave all 8 instances open at their time limits, so the values return to ordinary best-known status. Store-wide, every remaining optimality stamp now comes from the audited 4-run protocol. This family carries 40 certified TDVRP BKS (n=25), unchanged.

#### 2026-07-11

**40 BKS stamped proven optimal** (`metadata.optimality`) at n=25, under a full re-certification (stamps regenerated from scratch under a stronger protocol after a prover defect was found and fixed). Each stamp certifies 4 independent exact solves (cold and warm starts x 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp.

#### 2026-07-08

120 of 168 BKS improved (mean -1.04%, largest single improvement -2.77%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-07

19 BKS improved by exact solves and **proven optimal** by kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50. The R201–R211 n=25 and RC101–RC108 n=50 groups each collapse to a single TDVRP instance (identical customers, time windows ignored), so each group shares 1 proven optimum (4804.299826 and 9324.268643 respectively). Certificates: optimal under checker-exact route costs and standard LP/pricing tolerances, completeness modulo Lera epsilon dominance. The same campaign certified 40 of the other stored Dabia TDVRP n=25 BKS optimal as stored.

Structured optimality metadata: all 48 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object with the prover, certificate wording, proven optimum, dual bound and wall time (schema `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0).

#### 2026-07-06

All 168 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-05

Initial BKS population, 168/168: the TDVRP variant had no published or earlier heuristic solutions, so all BKS come from the initial large-scale seeding sweep across all 4 TD families (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-02

Instances and ATF sidecars populated (no BKS yet, because no historic TDVRP solutions exist for this material).
