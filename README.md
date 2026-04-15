# Hindered Rigid Rotor — Quantum Mechanics Simulation
**Quantum Mechanics · University of Central Florida**

---

## Overview

Numerical solution of the time-independent and time-dependent Schrödinger equations
for a particle on a ring under a six-well periodic potential. The Hamiltonian is:

    H = -(1/2I) d²/dθ² + V₀ sin²(3θ/2),   θ ∈ [0, 2π],   V₀ = 1,   I = 20

The six-fold potential creates tunneling-split eigenvalue multiplets, making this
a clean testbed for quantum tunneling and rotational localization.

---

## Physics

### Hamiltonian matrix elements

Expanding in the plane-wave basis φₙ(θ) = e^(inθ)/√(2π), the matrix elements are:

- **Diagonal** (m = n):      H_nn = n²/(2I) + V₀/2
- **Off-diagonal** (|m−n| = 3):  H_mn = −V₀/4
- All other entries are zero.

This follows from the Fourier decomposition of sin²(3θ/2) = 1/2 − cos(3θ)/2,
which only couples modes separated by Δn = ±3.

### Basis sizes

| N   | Matrix dimension | Basis functions |
|-----|-----------------|-----------------|
| 3   | 7 × 7           | n = −3, …, 3   |
| 10  | 21 × 21         | n = −10, …, 10 |
| 20  | 41 × 41         | n = −20, …, 20 |
| 100 | 201 × 201       | n = −100, …, 100 |

`numpy.linalg.eigh` is used (not `eig`) to exploit Hermitian structure and
guarantee real, sorted eigenvalues.

### Eigenvalue convergence

The first six eigenvalues converge rapidly. By N = 20 the values are
indistinguishable from N = 100 to five significant figures:

    N=3:   [0.2415, 0.3097, 0.3097, 0.7250, 0.8153, 0.8153]
    N=10:  [0.2216, 0.2223, 0.2223, 0.6247, 0.6247, 0.6391]
    N=20:  [0.2216, 0.2228, 0.2228, 0.6246, 0.6246, 0.6391]  ← converged
    N=100: [0.2216, 0.2228, 0.2228, 0.6246, 0.6246, 0.6391]

The near-degeneracy in pairs (states 2-3 and 4-5) reflects the six-fold
symmetry of the potential producing tunneling-split doublets.

### Eigenfunction reconstruction

Eigenfunctions on the real-space grid are reconstructed via:

    ψⱼ(θ) = Σₙ cₙʲ · e^(inθ) / √(2π)

where cₙʲ are the j-th column of the eigenvector matrix from `eigh`.
**Note:** eigenvector columns must index as `HvecsSort[:, j]`, not
`HvecsSort[j, :]`.

### Part 2 - wavepacket localization and tunneling

A localized initial state is built as a real linear combination of the first
three eigenstates (which span the lowest near-degenerate triplet):

    |ψ(0)⟩ = f₁ − (i/√2)f₂ − (i/√2)f₃

where f₁ = ψ₁ and f₂, f₃ are symmetry-adapted real combinations of the
degenerate pair ψ₂, ψ₃:

    f₂ = (ψ₂ − ψ₃) / 2
    f₃ = −i(ψ₂ + ψ₃) / 2

Time evolution is analytic:

    |ψ(t)⟩ = Σₖ cₖ · e^(−iEₖt) · |ψₖ⟩

The tunneling half-life is extracted as the first time the probability
in the initial well drops to 50% of its peak. This is repeated for
eigenstate groups (1-3), (4-6), and (7-9) where states remain below
the barrier top, and plotted against the energy gap to the barrier.

---

## Parameters

| Symbol | Value | Description                        |
|--------|-------|------------------------------------|
| V₀     | 1     | Potential amplitude                |
| I      | 20    | Moment of inertia                  |
| N      | 3, 10, 20, 100 | Basis truncation          |
| θ grid | 1000 points | Real-space visualization grid |

---

## Requirements

```bash
pip install numpy scipy matplotlib
```

---

## Usage

```bash
python part1_eigensolver.py   # eigenvalues, convergence plot, eigenfunctions
python part2_dynamics.py      # wavepacket evolution and tunneling half-life plot
```

---

## Key Results

- The potential V₀ sin²(3θ/2) has **six minima** at θ = 2πk/6, producing
  near-degenerate triplets in the spectrum from tunneling splitting.
- Basis convergence is essentially complete at N = 20 for the lowest six levels.
- The localized wavepacket built from states 1-3 tunnels between wells on a
  timescale set by π/(E₂ − E₁), the inverse of the tunneling splitting.
- States 4-6 form a second triplet just below the barrier; their tunneling
  time is much shorter due to the larger splitting.

> **Periodic boundary conditions** are automatic in the plane-wave basis.
> No special enforcement at θ = 0 / 2π is needed.

---

## Acknowledgments

Course project for Quantum Mechanics at the University of Central Florida.
Problem specification provided by Dr. Luca Argenti.
