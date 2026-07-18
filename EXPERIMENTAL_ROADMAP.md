# ParadigmForge — Experimental Roadmap

**Status: pre-execution.** This document is the execution plan for the empirical program
described (as *proposed* work) in the paper. **No ParadigmForge experiment has been run, and no
numerical result is reported anywhere in this project.** Every table in the paper marked
`[PENDING]` is filled only by the runs specified below, after the corresponding preregistration is
frozen and hash-pinned. This roadmap exists so that the architecture paper makes falsifiable,
executable commitments rather than open-ended promises.

The convention follows the companion Bourbaki projects: contributions and results are tagged
`[impl]` (built and exercised), `[design]` (specified, not built), `[planned]` (a preregisterable
protocol not yet run). At this checkpoint the ParadigmForge software layer is `[design]`; the
substrates it depends on (Mirador, ProofContext) are separately published `[impl]` prototypes.

---

## 0. Dependencies and blocking decisions

ParadigmForge is a layer on top of the existing Bourbaki substrates. Before any run:

| Dep | Provides | Current state | Blocking gap |
|-----|----------|---------------|--------------|
| Mirador | typed, versioned cells; identity; invalidation; event log | `[impl]` prototype (28 tests) | needs the ParadigmForge node/relation extensions of §5 wired in |
| ProofContext | dependency-aware retrieval; Evidence Bundles | `[impl]` prototype (21 tests) | **no cross-domain analogy retrieval; no collision-search client** (blocks BridgeForge, novelty stage 1) |
| TheoryForge | operator-guided candidate generation | `[design]` checkpoint | discovery operators not implemented |
| CongressBench | fresh-context adversarial review; FTPR | `[design]`/`[impl]` harness (synthetic reviewer only) | no wired formal backend; no funded reviewer panel |
| Formal backend | Lean 4 / exact-rational certificates | named, **not installed** | default tier = exact-rational certificate + independent checker |

**Blocking decisions (must be signed off before P4 freeze):**

- **D1** Compute / inference budget and the model families used for generation vs. review (must be
  ≥2 distinct families for independence).
- **D2** Selection and licensing of retrospective corpora for Track A (technique reconstruction) —
  each requires a *pre-discovery* knowledge cut that is defensible to a historian of mathematics.
- **D3** The identified external expert panel for blind adjudication (Tracks A–D) and their rubric
  sign-off.
- **D4** The held-out concept/bridge target sets for Tracks B and C, authored so their canonical
  abstraction was never published in the exposed context (contamination control).
- **D5** Which frozen versions of Mirador / ProofContext / CongressBench act as downstream
  agents, pinned by hash.
- **D6** The cross-domain analogy-retrieval and collision-search extensions of ProofContext
  (implementation owner and interface freeze).

---

## 1. Phase schedule

| Phase | Deliverable | Gate |
|------|-------------|------|
| **P0** | Extend Mirador schema with ParadigmForge node/relation types (§5); executable JSON Schemas + validators; zero-cost stub pipeline | schemas validate; stub round-trips |
| **P1** | Implement Frontier Atlas builder over a frozen corpus; produce a bottleneck portfolio for one documented problem | Atlas reproduces the documented bottlenecks of the pilot campaign |
| **P2** | Implement Experimentarium (finite enumeration, symbolic, counterexample search, invariant computation) with exact-arithmetic re-checking | counterexample search reproduces known counterexamples on seeded tasks |
| **P3** | Implement TechniqueForge / BridgeForge / ConceptForge operators + the necessity-ablation harness | each operator emits typed candidates ≤ `[EVIDENCE]`; ablation harness runs |
| **P4** | **Preregistration freeze**: protocols, corpora manifests, ablation matrix, novelty rubric weights, split hashes stamped into `FROZEN.md` + `MANIFEST.sha256` | freeze event exists in the Mirador log |
| **P5** | Run **Track A** (technique reconstruction, retrospective) | raw artifacts under `results/trackA/` |
| **P6** | Run **Track B** (bridge discovery) and **Track C** (concept formation) | raw artifacts; theorem-transport / held-out prediction checks |
| **P7** | Run governance ablations (independent review on/off; negative-knowledge on/off) with CongressBench | FTPR and cosmetic-novelty escape rates estimated |
| **P8** | Longitudinal **Track D** pilot (domain productivity) under external expert governance | ≥1 candidate language tracked across ≥2 problem families over a fixed horizon |
| **P9** | Sealed-test run (once) + paper results tables + reproducibility report + arXiv archive | no optional stopping; every table regenerates from raw |

Tracks A–C are the primary quantitative evidence; Track D is explicitly longitudinal and reported
qualitatively with pre-registered milestones, never as a single success rate.

---

## 2. ParadigmForgeBench task construction

### Track A — Technique reconstruction (retrospective)
- Candidate historical techniques (used **only** where the pre-discovery context can be cleanly
  reconstructed and the contamination risk is documented): diagonalization; generating functions;
  the probabilistic method; the polynomial method; Fourier-analytic techniques in additive
  combinatorics; monotonicity-quantity mechanisms in geometric flows. Each case must pass a
  historian-of-mathematics review that the exposed context genuinely precedes the technique.
- **Contamination is the first-order threat.** Every base technique is public and memorized by any
  modern LLM. Mitigations: (i) a memorization probe run *before* scoring, (ii) author-perturbed
  variants whose surface form was never published, (iii) scoring on *functional* reconstruction
  (does the emitted mechanism discharge the target obligation under ablation?), not textual match.
- **Pass criterion:** an ablation shows the reconstructed technique's novel component is
  *counterfactually necessary* to close the target lemma; a mere restatement of exposed context
  fails.

### Track B — Bridge discovery
- Provide two domains with a known (withheld) structural correspondence and a set of decoy pairs.
- **Pass criterion:** the system produces a typed alignment that (i) transports at least one
  nontrivial theorem with a checkable proof obligation, (ii) yields a testable prediction confirmed
  by Experimentarium, and (iii) emits a *bridge-failure certificate* stating where the translation
  breaks. Verbal analogy without transport is scored as failure.

### Track C — Concept formation
- Provide families of examples with the canonical invariant/abstraction withheld and a held-out
  split.
- **Pass criterion:** the proposed concept compresses the visible examples (fewer independent
  conditions) *and* predicts held-out behaviour above a preregistered baseline, and survives the
  overfitting probes of §4.

### Track D — Domain productivity (longitudinal)
- A candidate conceptual language is admitted only after Track C; it is then tracked over a fixed
  horizon for: number of independently derived nontrivial theorems, number of distinct problem
  families touched, internal (non-artificial) questions generated, transfer to an established domain,
  and use by an agent *other* than its proposer.
- **Why not a short benchmark:** "new branch" is defined by sustained autonomous productivity;
  the metric is a longitudinal trajectory with preregistered milestones, never a single scalar.

---

## 3. Baselines

Every track is run against, at minimum:

- **B-LLM** a strong language-model prover/reasoner prompted directly (best-of-N, with budget
  matched).
- **B-RET** a retrieval-only system (ProofContext Evidence Bundles, no generation operators).
- **B-DEBATE** unguided multi-agent debate without typed representation or the authority membrane.
- **B-TF** TheoryForge without the ParadigmForge layer (conjecture/reduction generation only).
- **PF** full ParadigmForge.

Comparisons are within a track and setting only; numbers are never compared across incompatible
settings. Compute and inference budgets are reported for every arm.

---

## 4. Novelty and anti-gaming controls

Each candidate is screened for, and scored against, the failure modes the novelty model of the
paper enumerates:

- cosmetic renaming (content-hash identity collision in Mirador → flagged non-novel);
- disguised restatement (semantic equivalence to an exposed cell);
- trivial generalization (no new theorem yield, no counterfactual necessity);
- memorized rediscovery (memorization probe; perturbed variants);
- benchmark contamination (held-out splits, sealed test run once);
- circular definitions (acyclicity-of-justification audit);
- analogy without transport (no transported theorem → BridgeForge reject);
- concepts fitted only to selected examples (held-out predictive test; perturbation robustness).

Novelty is reported as a **vector** (structural, operational, explanatory-compression, theorem
productivity, transfer breadth, counterfactual necessity, robustness, independent rediscovery,
formal-verification coverage, proof-complexity reduction, hypothesis-complexity reduction). A
scalar, if used at all, is only for scheduler allocation and is preregistered; it never decides
promotion.

---

## 5. Governance ablations

Remove one mechanism at a time and measure the change in False Theorem Promotion Rate (FTPR)
and cosmetic-novelty escape rate:

- − Frontier Atlas (undirected exploration);
- − negative knowledge (failures not stored/reused);
- − BridgeForge;
- − Experimentarium (no computational falsification pressure);
- − independent review (self-review only) — expected to *raise* FTPR (the central governance test);
- − Mirador typed representation (prose cells);
- − formal verification;
- − domain-transfer requirement for Track D.

The primary governance result the program seeks is whether the authority membrane + fresh-context
review measurably lowers FTPR and cosmetic-novelty escape relative to the ablations, on `n>1`
campaigns — the hypothesis CongressBench is built to test.

---

## 6. Metrics and statistics

- Primary per-track pass rates with **paired** comparisons on shared items (McNemar), clustered by
  base problem/domain; design effect `1+(m-1)ρ` reported; ICC estimated from a pilot before freeze.
- Effect sizes with 95% intervals, not only p-values; multiplicity controlled (Holm).
- FTPR per governance condition (never collapsed with discovery metrics into one score).
- Compute/inference budget per arm (tokens, wall-clock, dollars).
- Every table regenerates deterministically from raw artifacts; the sealed test runs exactly once
  with no optional stopping.

---

## 7. Open implementation tasks (tracked)

- [ ] Mirador schema extension: 15 node types, 14 relation types (§5 of the paper) + validators.
- [ ] ProofContext cross-domain analogy retrieval + collision-search client (blocks BridgeForge and
      novelty stage 1).
- [ ] Frontier Atlas builder + bottleneck-portfolio exporter.
- [ ] Experimentarium: exact-arithmetic core, counterexample engine, invariant computation,
      empirical-law detection.
- [ ] TechniqueForge / BridgeForge / ConceptForge operator library with the necessity-ablation
      harness.
- [ ] DomainFoundry longitudinal productivity ledger.
- [ ] Negative-knowledge store (FailedApproach / Obstruction / bridge-failure certificates) with
      reuse in the Frontier Atlas.
- [ ] Wire a formal backend (exact-rational certificate + independent checker as default; Lean 4
      optional/gated).
- [ ] CongressBench-compatible reviewer interface for ParadigmForge artifacts.
- [ ] ParadigmForgeBench task banks (Tracks A–D) + contamination controls + sealed sets.
- [ ] Preregistration freeze ritual (`FROZEN.md` self-hash + `MANIFEST.sha256`).

---

## 8. Honesty checklist (enforced before any results are reported)

1. No fabricated accuracy, success rate, table cell, expert quote, or discovered theorem.
2. Implemented vs. proposed components explicitly separated in every claim.
3. Rediscovery labelled as rediscovery; novelty claims gated on the anti-gaming controls of §4.
4. Negative results and null findings reported, not suppressed.
5. Every citation verified against a primary source (unverifiable ones omitted).
6. Sealed test executed once; no optional stopping; raw artifacts archived.
