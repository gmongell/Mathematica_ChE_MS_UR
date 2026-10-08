# Rochester chemical-engineering and applied-physics notebooks

A historical Mathematica collection spanning mathematical methods, statistical/thermodynamic calculations, and polarization optics. Topics below are based on inspected cells rather than assumptions about course numbers.

## Functions and engineering applications

| Notebooks | Observed content | Inferred application |
|---|---|---|
| `ChE400_FourierSeries.nb`, `ChE400_Sqwv.nb`, `ChE400_Sincos.nb` | Fourier expansions and periodic-function experiments | Spectral representations for transport and boundary-value models [1] |
| `ChE400_newtheat.nb`, `ChE400_PlatonianReactorMeltdown_Final.nb`, other `ChE400_*` | Mathematical-method exercises, including heat/diffusion-reaction-style models | Studying eigenmodes, transient behavior, and analytical limits [1] |
| `ChE413.nb` | Lennard-Jones potential calculations and entropy-related exercises | Connecting microscopic potentials with thermodynamic reasoning |
| `ChE418.nb` | Energy, entropy/logarithms, and Boltzmann-factor expressions | Statistical-mechanics model exploration |
| `ChE441.nb` | Symbolic logarithmic/integral equations and algebraic solving | Analytical manipulation of course models; full topic coverage is not inferred |
| `ChE447.nb` | `LinHoriz` and `Waveplate[delta, rho]` polarization matrices | Polarizer/retarder calculations in a Stokes/Mueller representation [2] |

## Example function calls

Use Mathematica with a front end; these are notebooks with shared kernel state, not an installed package. Set the working directory to the clone root, then:

```wolfram
nb = NotebookOpen[FileNameJoin[{Directory[], "ChE447.nb"}]];
```

Evaluate the cells defining `LinHoriz` and `Waveplate` in the notebook, then run:

```wolfram
Dimensions[Waveplate[Pi/2, Pi/4]]
Waveplate[Pi/2, Pi/4] . {1, 1, 0, 0}
```

Both arguments are angles in radians: retardance and orientation. The first expression should identify a 4-by-4 matrix; the second applies it to an illustrative Stokes vector under the notebook’s convention. These calls were matched to definitions but not executed in a Wolfram runtime during this review.

## Reproducibility and earlier documentation

Start a fresh kernel for each notebook and evaluate actual input cells in dependency order. Some narrative prose is stored as Input cells and should not be evaluated as code. Preserve parameter units, boundary/initial conditions, and the distinction between stored output and newly evaluated results. Duplicate and differently numbered notebooks are historical variants, not a tested release sequence.

`README_Course_Notebooks.md` is retained as historical documentation, but its generic mappings (for example, ChE418 to controls and ChE447 to reaction engineering) do not match the inspected cells. It also describes capabilities not established by this review. Use this source-based index when choosing a notebook. The repository is educational/research material, not a qualified engineering design calculation.

## Review scope and software citation

Documentation reviewed on 2026-10-08 against source commit [`e4b8161ef894`](https://github.com/gmongell/Mathematica_ChE_MS_UR/tree/e4b8161ef894a7c2ee28d7a6fcaec28d9311b79a). “Observed” means supported by source inspection; engineering applications are reasoned possibilities unless explicitly demonstrated. Scholarly references provide methodological context and do not certify these implementations. Runtime validation is stated separately above.

For software attribution, cite Guy Francis Mongelli, *Mathematica_ChE_MS_UR*, the [repository](https://github.com/gmongell/Mathematica_ChE_MS_UR), the exact commit used, and your access date. Also cite the relevant method publications and any original third-party contributors. No unverified software DOI or release version is assigned by this documentation.

## Scholarly references

1. P. K. Kythe, M. R. Schäferkotter, and P. Puri (2003). *Partial Differential Equations and Boundary Value Problems with Mathematica*, 2nd ed., Chapman & Hall/CRC, ISBN 1584883146. [Wolfram bibliographic record](https://www.wolfram.com/books/search.html?year=2003). Scholarly background for symbolic and numerical boundary-value methods.

2. R. Clark Jones (1941). “A New Calculus for the Treatment of Optical Systems. I. Description and Discussion of the Calculus.” *JOSA* 31, 488–493. [DOI: 10.1364/JOSA.31.000488](https://doi.org/10.1364/JOSA.31.000488). Foundational polarization-matrix context; the inspected waveplate notebook uses a 4-by-4 Stokes/Mueller representation, not a 2-by-2 Jones matrix.

## Ownership and existing license notices

Copyright (c) 2025 Guy Francis Mongelli

The existing project notice declares Apache License 2.0 for project code. Documentation, prose, and figures are declared CC BY 4.0; notebook code cells are Apache-2.0 and narrative/figures CC BY 4.0. Preserve all file-level and third-party notices. This README update does not change ownership or licensing terms.


## Reproducibility, source verification, and contribution policy (2026-10-08)

- **Observed versus proposed:** The function inventory above describes inspected source where indicated. Engineering applications identified as *inferred* are potential uses, not verified features or validated performance claims.
- **Usage examples:** Treat the documented commands and calls as illustrative until the referenced source file, runtime version, dependencies, required input data, and working directory have been checked. Do not execute notebook fragments or batch-scheduler directives as standalone programs without adapting their context.
- **Scientific citations:** References above identify relevant governing methods and computational background; citing a publication does not imply that its algorithm is implemented in this repository or that the publication was authored by this repository owner.
- **Missing or null artifacts:** Empty, placeholder, missing, or non-executable source files must not be represented as functional implementations. Candidate restorations from personal archives require provenance, content comparison, license review, and explicit verification before committing code.
- **Access model:** This repository is publicly readable. Public visibility does not grant anonymous push rights; write access is controlled separately through repository collaborators, credentials, apps, deploy keys, and branch rules. This README is descriptive and does not itself enforce permissions.
