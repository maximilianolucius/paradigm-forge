# ParadigmForge

**A Governed Architecture for Machine-Generated Mathematical Theory Formation**

[![DOI](https://zenodo.org/badge/doi/10.5281/zenodo.21435135.svg)](https://doi.org/10.5281/zenodo.21435135)
&nbsp;License: text/figures [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

Author: Maximiliano Lucius

- **Paper (archival, DOI):** <https://doi.org/10.5281/zenodo.21435135>
- **All versions (concept DOI):** <https://doi.org/10.5281/zenodo.21435134>

This repository contains the LaTeX source of the ParadigmForge paper — an *architecture and
evaluation-framework* paper that extends the Bourbaki Engine research program from theorem
*promotion* to governed **theory formation**: the generation, testing, refinement, and adversarial
validation of candidate mathematical *techniques*, *cross-domain bridges*, *conceptual languages*,
and *candidate domains*.

> **Status.** This is a design-and-protocol paper. ParadigmForge is *proposed*; it builds on the
> separately published Mirador and ProofContext prototypes. **No ParadigmForge experiment has
> been run and no empirical result is claimed.** Result tables are explicit `[PENDING]`
> placeholders; the execution plan lives in [`EXPERIMENTAL_ROADMAP.md`](EXPERIMENTAL_ROADMAP.md).

## What ParadigmForge is (and is not)

- **Is:** a governed architecture separating a high-temperature *speculative zone* (Frontier Atlas,
  TechniqueForge, BridgeForge, ConceptForge, Experimentarium, DomainFoundry) from a near-zero
  temperature *trust zone* (Mirador, formalization, CongressBench review), joined by a one-way
  authority membrane; a typed epistemic representation for techniques/bridges/concepts/failures; a
  multidimensional novelty model; and ParadigmForgeBench, a four-track evaluation framework.
- **Is not:** a system that solves any open problem, "creates new mathematics", or reaches
  human-level mathematical creativity. It does not claim to solve a Clay Millennium Prize Problem;
  those are discussed only as a long-term motivating horizon.

## Repository layout

```
paradigm-forge/
├── paradigmforge.tex         # the paper (compiles to paradigmforge.pdf)
├── references.bib            # bibliography (primary-source verified)
├── figures/                  # TikZ figure sources (\input by main.tex)
│   ├── architecture.tex
│   ├── lifecycle.tex
│   ├── novelty-levels.tex
│   ├── bench-tracks.tex
│   └── casestudy-graph.tex
├── README.md
├── EXPERIMENTAL_ROADMAP.md   # full proposed-experiment execution plan
└── *.md                      # summaries of the companion Bourbaki papers (context)
```

## Building

Requires a standard TeX distribution (TeX Live 2021+). Only standard packages are used
(`amsmath`, `amssymb`, `amsthm`, `booktabs`, `tikz`, `hyperref`, `natbib`, `geometry`, …); no
proprietary packages.

```bash
# with latexmk (recommended)
latexmk -pdf paradigmforge.tex

# or manually
pdflatex paradigmforge.tex
bibtex   paradigmforge
pdflatex paradigmforge.tex
pdflatex paradigmforge.tex
```

The output is `paradigmforge.pdf`.

## Relationship to the Bourbaki Engine

| Component | Role | ParadigmForge relationship |
|-----------|------|----------------------------|
| MathIngestBench | evaluates ingestion of the literature into cells | supplies the corpus the Frontier Atlas reads |
| Mirador | typed, versioned claims / dependencies / provenance / epistemic state | **extended** with theory-formation node & relation types |
| ProofContext | retrieves the exact context to use a result safely | source of Evidence Bundles; must gain analogy-retrieval + collision search |
| TheoryForge | generates conjectures, reductions, definitions, programs | ParadigmForge is the layer *above* it (techniques, bridges, concepts, domains) |
| CongressBench | evaluates adversarial, independent review (FTPR) | the review gate for ParadigmForge artifacts |
| Bourbaki Engine | governs the discovery → verification → promotion pipeline | ParadigmForge is its conceptual-evolution extension |

## Extending the work

1. Implement the Mirador schema extension (paper §5 / roadmap §7).
2. Add analogy-retrieval and collision search to ProofContext (unblocks BridgeForge).
3. Build the Experimentarium exact-arithmetic core.
4. Follow the phase schedule (`EXPERIMENTAL_ROADMAP.md`) and freeze the preregistration before any
   sealed run.

## Citation

If you refer to this work, please cite the archived version (DOI
[10.5281/zenodo.21435135](https://doi.org/10.5281/zenodo.21435135)) and the companion Bourbaki
papers listed in `references.bib`.

```bibtex
@misc{lucius2026paradigmforge,
  author       = {Lucius, Maximiliano},
  title        = {{ParadigmForge: A Governed Architecture for
                   Machine-Generated Mathematical Theory Formation}},
  year         = {2026},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.21435135},
  url          = {https://doi.org/10.5281/zenodo.21435135}
}
```

## License

Text and figures: CC BY 4.0. Any code added under the roadmap should follow the companion
projects' MIT convention.
