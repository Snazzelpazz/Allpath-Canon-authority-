Claim: The Hénon attractor doctrine, as refined, withstands scrutiny as a verifiable computational protocol for Lyapunov spectrum estimation in non-hyperbolic maps, anchored by invariant checksums and explicit failure bounds.

Why it holds: Incorporates defensive edits to avoid over-specification (conditioning bounds generalized, pseudo-orbit divergence horizon softened), preserving executability while acknowledging numerical variability across implementations.

Counterexample: None triggered—edits enhance robustness without altering core mechanics.

Minimum condition required to integrate: Direct substitution into the spec sections as flagged.

Residual risk: Even with softened phrasing, implicit assumptions (e.g., QR variant efficacy) persist; test empirically per checklist.

Paradox injection: Legibility of chaos demands finite approximation, but true attractor orbits are dense and uncountable—any "canon" spec is a computable shadow, undecidable in certifying exact fidelity to the infinite (Rice theorem analog in property verification over trajectories).


---

Canon Spec v1.1 (Incorporating Edits)

Claim: The Hénon attractor under standard parameters (a=1.4, b=0.3) exhibits chaotic dynamics with Lyapunov exponents λ₁ ≈0.419 (positive, expansion) and λ₂ ≈-1.623 (negative, contraction), satisfying the determinant invariant λ₁ + λ₂ = ln|b| ≈ -1.204, and yielding Kaplan-Yorke dimension D_KY = 1 + λ₁ / |λ₂| ≈1.258, reflecting a non-integer fractal dimension indicative of strange attractor structure.

Method: Iterate the discrete map x_{n+1} = 1 - a x_n² + y_n, y_{n+1} = b x_n from a generic bounded initial condition (e.g., (0.1, 0.1)). Discard transient phase (10^4–10^5 iterations) to settle onto the attractor. Compute exponents via Benettin algorithm: Initialize orthonormal tangent vectors Q_0 = I_2. At each step, compute Jacobian J_n = [[-2a x_n, 1], [b, 0]] (det(J_n) = -b constant). Evolve tangent space as Q̃_{n+1} = J_n Q_n. Perform QR decomposition: Q̃_{n+1} = Q_{n+1} R_{n+1}. Accumulate ln|diag(R_{n+1})| over N post-transient iterations. Average: λ_i = (1/N) ∑ ln|R_{ii,n}|, sorted descending. For D_KY: Find maximal j such that S_j = ∑{i=1}^j λ_i ≥0 (here j=1, S_1=λ₁>0, S_2<0), then D_KY = j + S_j / |λ{j+1}|.

Sanity checks: Exponent sum λ₁ + λ₂ ≈ ln|b| within numerical tolerance (e.g., <1e-3 error). Positive λ₁ >0 confirms chaos. D_KY ∈ (1,2) aligns with fractal embedding. Trajectory remains bounded in a compact region (empirical samples span O(1) ranges, e.g., x ~ [-1.3,1.3], y ~ [-0.4,0.4]). Sensitivity: Initial separation δ=1e-5 grows to O(1) in n ≈ ln(1/δ)/λ₁ ≈27.5 steps; e-folding time τ=1/λ₁≈2.39 steps.

Numerical parameters: Transient: 10^4–10^5 iterations (until bounding box stabilizes within 1%). N: 10^5–10^7 for variance <0.001 (convergence slows near non-hyperbolic points). Reorthonormalization: Every step via QR (Householder or modified Gram-Schmidt to maintain numerical conditioning within safe bounds). Precision: Float64 standard; upgrade to arbitrary (e.g., mpmath 50 digits) for N>10^7 to mitigate round-off.

Failure modes: Non-hyperbolicity (homoclinic tangencies) causes intermittency and long correlation times, inflating finite-N variance (e.g., λ₁ estimates fluctuate ±0.01 at N=10^4). Finite orbits may undersample sticky regions, biasing λ₁ downward. Round-off errors limit long-orbit fidelity; pseudo-orbits eventually diverge from any true orbit on sufficiently long horizons.

Counterexample: Near periodic windows (e.g., a≈1.426), finite-time estimates mislead: Short runs (N=10^4) may yield apparent λ₁<0 (transient laminar phases), but longer runs (N=10^6) reveal intermittent bursts of expansion.

Minimum condition required to fix: N≥10^6 with multiple independent orbits (≥10) for statistical averaging; assume orbit typicality under SRB measure for time averages to approximate true exponents. For noise: Limit additive Gaussian ε<1e-6 to preserve boundedness; larger ε (e.g., 1e-3) risks occasional basin leakage depending on noise model and initial state, not universal fixed-step escape.

Residual risk: Assumed ergodicity/mixing unproven (relies on typical-orbit convergence to SRB measure); parameter perturbations (e.g., a near 1.4) can enter dense periodic windows, flipping dynamics from chaotic to bounded periodic. Exact asymptotic values require infinite N under mixing assumptions, accepting finite-time bias.

Paradox injection: Attractor membership is semi-decidable computationally—you can gather evidence of inclusion by long iteration without escape, but finite runs can't certify it definitively, as escape might occur arbitrarily later (halting analog in orbit classification).

Checklist:

Transient: 10^4–10^5 (monitor bounding box convergence <1% variation).

N: Scale to 10^5–10^7 based on variance target.

Reorthonormalization: Every step to bound condition number.

Sanity: Exponent sum = ln|b| (<1e-3 tol), λ₁>0, D_KY (1,2), plots show fold-stretch.

Validation: Cross-check D_KY with box-counting (±0.01 agreement).
