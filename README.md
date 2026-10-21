[![INFORMS Journal on Computing Logo](https://INFORMSJoC.github.io/logos/INFORMS_Journal_on_Computing_Header.jpg)](https://pubsonline.informs.org/journal/ijoc)

# Subgame Perfect Methods in Nonsmooth Convex Optimization

This archive accompanies research published in the
[INFORMS Journal on Computing](https://pubsonline.informs.org/journal/ijoc).
The software and associated documentation are distributed under the
[MIT License](LICENSE).

This repository contains the computational materials for
[Subgame Perfect Methods in Nonsmooth Convex Optimization](https://doi.org/10.1287/ijoc.2025.1802)
by Benjamin Grimmer and Alex L. Wang.

The software was developed in the
[NonsmoothSubgamePerfectMethods repository](https://github.com/bgrimmer/NonsmoothSubgamePerfectMethods). Please go there if you would like to get a more recent version or would like support

## Cite

To cite the contents of this repository, please cite both the paper and this repo, using their respective DOIs.

https://doi.org/10.1287/ijoc.2025.1802

https://doi.org/10.1287/ijoc.2025.1802.cd

Below is the BibTeX for citing this snapshot of the repository.

```bibtex
@misc{GrimmerWang2026Code,
  author =        {Benjamin Grimmer and Alex L. Wang},
  publisher =     {INFORMS Journal on Computing},
  title =         {{Subgame Perfect Methods in Nonsmooth Convex Optimization}},
  year =          {2026},
  doi =           {10.1287/ijoc.2025.1802.cd},
  url =           {https://github.com/INFORMSJoC/2025.1802},
  note =          {Available for download at https://github.com/INFORMSJoC/2025.1802},
}
```

## Description

This software implements and compares methods for nonsmooth convex optimization
using subgradient and proximal operator oracles. It supports the numerical
experiments in Section 5 and Figure 1 of the paper.

The implementations include the Kelley cutting-plane-Like Method (KLM), the
Subgame Perfect Proximal Point Algorithm (SPPPA), and full-memory, fixed-window,
and active-bundle variants. The experiments compare these methods with
subgradient and Optimized Proximal Point Algorithm (OPPA) baselines, including
experiments with inexact proximal evaluations.

The test problems are generated synthetically by the supplied Julia code;
no external datasets are needed.

## Repository Contents

The following paths assume the original development repository's flat layout.

| File | Purpose |
| --- | --- |
| `BasicDefinitions_v5.jl` | Problem and oracle definitions, synthetic instance generators, common method interfaces, and baseline routines. |
| `KLM.jl` | KLM with full or fixed-window memory. |
| `KLM_active.jl` | KLM with an active-bundle memory rule. |
| `SPPPA.jl` | SPPPA with full or fixed-window memory. |
| `SPPPA_active.jl` | SPPPA with an active-bundle memory rule. |
| `OPPA.jl` | OPPA baseline. |
| `easy_KLM.jl` | An additional KLM variant that switches from planning subproblems to simpler subgradient steps; not included by the main notebook. |
| `NumericalTest.ipynb` | Experiment driver, inexact-oracle error tracking, result summaries, and plotting. |
| `p1.png` through `p4.png` | Plot exports included in the development repository. |

## Building

The code is written in Julia and runs through a Jupyter notebook with an IJulia
kernel. The notebook metadata records Julia **1.9.1**; this records the saved
kernel version, rather than a tested compatibility range.

The external Julia packages used by the code are `Convex`, `SCS`,
`MathOptInterface`, and `Plots`. `IJulia` provides the notebook interface.
`LinearAlgebra`, `Random`, and `Printf` are Julia standard libraries.

From the repository root, start Julia with a local project environment:

```sh
julia --project=.
```

Then install the packages and open the notebook interface:

```julia
using Pkg
Pkg.add(["Convex", "SCS", "MathOptInterface", "Plots", "IJulia"])

using IJulia
IJulia.notebook(dir=pwd())
```

If IJulia offers to install Jupyter, accept that installation or use an existing
Jupyter installation. See the official
[IJulia installation instructions](https://ijulia.org/stable/manual/installation/)
and [running instructions](https://ijulia.org/stable/manual/running/).

The source snapshot does not contain a `Project.toml` or `Manifest.toml`.
Consequently, the commands above install dependencies but do not reconstruct
the original package versions.

<!-- AUTHOR CHECK: Before finalizing the archive, include the tested
Project.toml and Manifest.toml, record the operating system and an approximate
runtime, and replace Pkg.add(...) above with Pkg.instantiate(). -->

## Results

Figure 1 in the paper presents four panels:

| Panel | Notebook plot | Experiment |
| --- | --- | --- |
| Top left | `p1` | Subgradient methods on a structured max-affine problem. |
| Top right | `p2` | Proximal methods on a low-rank least-squares problem with exact proximal evaluations. |
| Bottom left | `p3` | Proximal methods on a structured LASSO problem with inexact proximal evaluations. |
| Bottom right | `p4` | Accumulated inexactness terms for the LASSO experiment. |

The experiments compare full-memory, recent-memory, and active-bundle variants.
The notebook displays the combined figure and prints summaries of the final
objective gaps, best objective gaps, elapsed times, and accumulated errors where
applicable.

## Replicating

Open `NumericalTest.ipynb` with a Julia kernel and keep the working directory
at the repository root so that its relative `include(...)` paths resolve.
Start with a fresh kernel and execute the first code cell, which loads the
implementations and defines the experiment routines.

Then execute the experiment cell. The following equivalent call makes the
settings described in Section 5 explicit and saves the combined figure:

```julia
mkpath("results")

results = run_updated_tests(
    d=200,
    m_pts=120,
    max_iters=80,
    k_mem=10,
    seed=42,
    inexact_inner_iters=80,
    cert_iters=1000,
    rotated_L=5.0,
    rotated_h_min=1e-2,
    rotated_h_max=1e3,
    plot_best_gaps=false,
    savefig_path="results/figure1.png",
)
```

This call requests a combined plot at `results/figure1.png`. The returned
`results` value contains objective and timing histories for the three problems
and inexactness histories for the LASSO experiment. The individual problem
generators use seeds 1, 3, and 4, respectively.

## Ongoing Development

The IJOC repository preserves the version associated with the paper.
Subsequent development is maintained at
[NonsmoothSubgamePerfectMethods](https://github.com/bgrimmer/NonsmoothSubgamePerfectMethods).

## Support

For questions or bug reports, please
[open an issue in the development repository](https://github.com/bgrimmer/NonsmoothSubgamePerfectMethods/issues).
Author contact information is provided in [AUTHORS](AUTHORS).
