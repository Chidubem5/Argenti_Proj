# Argenti_Proj

This project numerically solves the time-independent and time-dependent Schrödinger equations for a hindered rigid rotor — a particle on a ring subject to a periodic potential. The Hamiltonian is:

Ĥ = −(1/2I) d²/dθ² + V₀ sin²(3θ/2), θ ∈ [0, 2π], V₀ = 1, I = 20

The system models rotational tunneling and quantum localization in a six-well potential landscape, with direct relevance to molecular torsional dynamics and periodic lattice problems.

Physics summary
Part 1 — eigenvalue problem

Eigenstates are expanded in the plane-wave basis:

φₙ(θ) = e^(inθ) / √(2π), n = −N, …, N

The Hamiltonian matrix is constructed analytically. Kinetic energy contributes diagonal terms n²/(2I); the potential V₀ sin²(3θ/2) couples modes separated by ±3 in wavenumber. Diagonalization via numpy.linalg.eigh yields eigenpairs. Convergence of the first six eigenvalues is tracked across N = 3, 10, 20, 100.
Part 2 — wavepacket dynamics

A localized initial state is constructed as a linear combination of low-lying eigenstates weighted to maximize probability density near a single potential minimum. Time evolution proceeds analytically:

|ψ(t)⟩ = Σ cₖ e^(−iEₖt/ℏ) |ψₖ⟩

Tunneling timescales are extracted by measuring when the wavepacket probability in the initial well drops to half its peak value. This is repeated for eigenstate groups (1–3), (4–6), and (7–9) where applicable, and the tunneling time is plotted against the energy gap to the barrier top.
