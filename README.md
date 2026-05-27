# Source Code for "Mind the Gap: Mixtures of Gaussians in Approximate Differential Privacy" (ICML 2026)

**Authors:** Huikang Liu, Aras Selvi, and Wolfram Wiesemann

**Affiliations:** Shanghai Jiao Tong University; UCL School of Management; Imperial Business School

**Corresponding authors:** Huikang Liu (`hkl1u@sjtu.edu.cn`), Aras Selvi (`a.selvi@ucl.ac.uk`), and Wolfram Wiesemann (`ww@imperial.ac.uk`)

This folder contains the Julia implementation accompanying the paper. It
calibrates and compares:

- the multi-Gaussian mechanism;
- the quasi-Gaussian mechanism; and
- the analytic Gaussian mechanism of Balle and Wang (2018).

## Run The Example

Required Julia packages: `Distributions`, `StatsBase`, `Optim`, `Roots`,
`QuadGK`, and `SpecialFunctions`.

Edit the first three inputs in `example.jl`:

```julia
epsilon = 0.5
delta = 1e-3
sensitivity = 1.0
```

Then run:

```bash
julia example.jl
```

The example reports scalar-noise utility quantities for noise `Z`:

- `L1 noise = E[|Z|]`, called amplitude in the research code;
- `L2 noise = sqrt(E[Z^2])`, the root-mean-square magnitude; and
- `L2 squared noise = E[Z^2]`, called power in the research code.

## Files

`multi.jl` is the public multi-Gaussian entry point. It uses the same
multi-Gaussian mechanism throughout, while selecting numerical routines based
on the privacy-parameter region:

- `multi_gaussian_low_epsilon_small_delta.jl` is used when
  `epsilon <= 0.5` and `delta <= 1e-3`. This difficult boundary regime uses
  tighter integration and root-finding tolerances.
- `multi_gaussian_high_epsilon.jl` is used when `epsilon >= 5`. In this
  regime, mixture weights can be extremely concentrated; the implementation
  uses log-domain arithmetic and localized integration windows for numerical
  stability and speed.
- `multi_gaussian_default.jl` is used for all remaining parameter values. It
  uses direct finite-mixture quadrature with the calibrated shift-grid check.

`quasi.jl` similarly uses `quasi_gaussian_low_epsilon_small_delta.jl` when
`epsilon <= 0.5` and `delta <= 1e-3`, because the calibration boundary
benefits from tighter bisection tolerances. It uses
`quasi_gaussian_default.jl` for all remaining values. These quasi-Gaussian
files implement the same mechanism and differ only in numerical calibration
tolerances.

`analytic_gaussian.jl` contains the analytic Gaussian baseline.

## Attribution

`analytic_gaussian.jl` is a Julia translation of the analytic Gaussian example
implementation by Borja Balle:

https://github.com/BorjaBalle/analytic-gaussian-mechanism

That upstream implementation is distributed under the Apache License 2.0 and
implements the analytic calibration method of Balle and Wang (ICML 2018).


## Note

This repository included the source codes of the DP mechanisms proposed in the paper. Please reach out to the authors for the experiments that rely on these mechanisms (including the ML experiments in the appendices), as well as for the cluster computing codes.
