# Chemical Engineering Course Notebooks (ChE 413 / 418 / 441 / 447)

This repository aggregates four Wolfram *Mathematica* notebooks organized by course number for core chemical engineering topics. The notebooks are designed for **instruction**, **self-study**, and **research-grade calculation**, with a focus on reproducibility and exportable figures/tables.

> **Audience.** Upper-division undergraduates, graduate students, and practitioners seeking concise, executable exemplars in transport, control, thermodynamics, and reaction engineering.
>
> **Note.** Course numbering/titles vary by institution. The topical mappings below reflect common curricula; adjust the one-line summaries to match your syllabus if needed.

---

## Repository Contents

- `ChE413.nb` — *Transport Phenomena / Momentum–Heat–Mass Transfer*  
  Boundary-value and initial-value formulations; nondimensionalization (Re, Pr, Sc, Pe, Nu, Sh); canonical solutions for laminar flow/heat/mass transfer; eigenfunction expansions and PDE solvers; verification against limiting regimes.

- `ChE418.nb` — *Process Dynamics & Control*  
  Linearization about operating points; Laplace-domain models and transfer functions; step/impulse responses; stability margins; PID design/tuning workflows; frequency-domain analysis (Bode/Nyquist); setpoint tracking and disturbance rejection examples.

- `ChE441.nb` — *Thermodynamics & Phase Equilibria*  
  Residual/ideal properties; cubic EOS (PR/SRK) workflows; fugacity and departure functions; activity-coefficient models (NRTL/UNIQUAC) for VLE/LLE; flash calculations (isothermal/isobaric/adiabatic); consistency checks and sensitivity analyses.

- `ChE447.nb` — *Chemical Reaction Engineering*  
  Kinetics parameter estimation and model discrimination; reactor models (CSTR/PFR/PBR); nonisothermal energy balances; heat/mass transfer limitations and effectiveness factor; residence-time distributions; multiple steady states and stability.

If you prefer a tidy layout, move all notebooks into `notebooks/` and adopt the optional structure below.

---

## Quick Start

1. **Requirements**
   - Wolfram *Mathematica* **14.x** recommended (13.x generally compatible).
   - No external paclets required for core workflows; optional plotting/units paclets may be used and can be installed on demand:
     ```wolfram
     PacletInstall["MaTeX"]
     ```

2. **Clone**
   ```bash
   git clone <repo-url>.git
   cd <repo-name>
   ```

3. **Open a notebook**
   - Launch *Mathematica*, open any `.nb` file.
   - Evaluate the environment cell first (if present):
     ```wolfram
     SetDirectory@NotebookDirectory[];
     $HistoryLength = 0;
     ```

4. **Run & export**
   - Evaluate in order: *Setup → Parameters/Data → Computation → Plots/Export*.
   - Exported assets will be written to `./output/` if the notebook includes export cells.

---

## Recommended Folder Structure (Optional)

```
.
├── notebooks/
│   ├── ChE413.nb
│   ├── ChE418.nb
│   ├── ChE441.nb
│   └── ChE447.nb
├── data/
│   └── (CSV/JSON tables, property databases, experimental datasets)
├── src/
│   └── (shared .wl/.m utilities for EOS, transport, kinetics, control)
└── output/
    ├── figures/
    └── tables/
```

Create output paths programmatically inside a notebook:
```wolfram
SetDirectory@NotebookDirectory[];
dirs = FileNameJoin /@ {{".","output"},{"." ,"output","figures"},{"." ,"output","tables"}};
Scan[If[!DirectoryQ[#], CreateDirectory[#, CreateIntermediateDirectories -> True]] &, dirs];
```

---

## Reproducibility Guidance

- **Units:** Be explicit and consistent (SI recommended). Wrap degree-based trig as `Cos[θ Degree]` to avoid radian/degree mismatches.
- **Seeds:** If random sampling appears, call `SeedRandom[...]` before generation.
- **Precision:** Increase `WorkingPrecision` when solving stiff ODE/PDEs or near singularities.
- **Assertions:** Use `VerifyTest` or `Assert`-style checks for parameter ranges and mass/energy balance closure.
- **Performance:** Set `$HistoryLength = 0` to reduce memory overhead during long sweeps.

---

## Typical Computation Blocks (Patterns)

**Transport (ChE413)**
```wolfram
(* Dimensionless form and solution sketch *)
ClearAll[u, x, t, L, α];
pde = D[u[x,t],t] == α D[u[x,t],{x,2}];
ic  = u[x,0] == u0[x];
bc  = {u[0,t] == UA, u[L,t] == UB};
sol = NDSolveValue[{pde, ic, bc}, u, {x,0,L}, {t,0,tmax}];
Plot3D[Evaluate[sol[x,t]], {x,0,L}, {t,0,tmax}, Mesh->None]
```

**Control (ChE418)**
```wolfram
sys = TransferFunctionModel[{{{k}}, {{1, τ}}}, s];
pid = PIDTune[sys, "PID"];
res = OutputResponse[FeedbackConnect[pid, sys], UnitStep[t], t];
```

**Thermo (ChE441)**
```wolfram
(* PR EOS fugacity coefficient φ for a mixture; sketch *)
(* Use EOS parameter functions a(T), b, α(T,κ), mixing rules *)
(* Compute φ_i and perform isothermal flash with Rachford–Rice *)
```

**Reactor (ChE447)**
```wolfram
(* Nonisothermal CSTR with Arrhenius kinetics *)
ode = {
  V cA'[t] == F(cAin - cA[t]) - V k0 Exp[-Ea/(R T[t])] cA[t],
  ρ Cp V T'[t] == F ρ Cp (Tin - T[t]) + (-ΔH) V k0 Exp[-Ea/(R T[t])] cA[t] - UA (T[t]-Tc)
};
ss  = Solve[0 == ode /. Derivative[1][#][t]->0& /@ {cA,T}, {cA[t],T[t]}];
```

---

## Data & Property Handling

- **Thermodynamics:** Store EOS parameters and binary-interaction coefficients (`kij`) in `data/` (CSV/JSON). Load via `Import[...]` and validate with simple test points.
- **Transport correlations:** Keep correlations (e.g., Sieder–Tate, Chilton–Colburn) in helper functions under `src/` with a consistent interface and unit checks.
- **Kinetics:** Maintain a parameter table (pre-exponential factors, activation energies, stoichiometry) and provide uncertainty bounds where available.

---

## Testing & Validation (Optional)

- Compare transport solutions against textbook limiting cases.
- Validate control tuning against second-order templates and gain/phase margins.
- Cross-check EOS results using alternative packages or reference datasets.
- Perform mass/energy balance closure checks and reactor stability analysis.

---

## Version Control Tips

- Enable **Git LFS** for binary notebooks:
  ```bash
  git lfs install
  echo "*.nb filter=lfs diff=lfs merge=lfs -text" >> .gitattributes
  ```
- Keep `output/` out of version control using `.gitignore`.
- Consider extracting reusable kernels into `src/*.wl` for easier diffing and unit tests.

Example `.gitignore`:
```
/output/
/data/raw/
*.DS_Store
```

---

## Academic Integrity

These materials support learning and research. If used in a course, follow your institution’s collaboration and citation policies. Include attributions in reports and presentations as appropriate.

---

## License

Add a `LICENSE` file (e.g., MIT, BSD-3-Clause, CC-BY-4.0) to clarify reuse terms. If omitted, all rights reserved by default.

---

## Citation

Optionally add `CITATION.cff`:
```yaml
cff-version: 1.2.0
title: "Chemical Engineering Course Notebooks (ChE 413 / 418 / 441 / 447)"
authors:
  - family-names: Mongelli
    given-names: Guy Francis
version: 0.1.0
date-released: 2025-08-14
```

---

## Contact

For questions or suggestions, open an Issue in this repository.
