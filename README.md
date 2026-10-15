# Footprint

Symbolic verification of the Ising and Fibonacci footprint computations that
appear in Appendix D of my Ph.D. thesis, *An application of the footprint
approach to quantum error correction in fusion systems, with a view to
topological quantum computing*.

The notebooks in this repository re-derive, symbolically and reproducibly, the
fusion-channel probabilities used in the motivating case studies of Chapter 5
of the thesis. They are provided as a companion to the thesis, not as a
standalone project.

## What this repository contains

- **`IsingFootprint.ipynb`** — symbolic verification of the 4-Ising and
  6-Ising footprint computations. Builds the $F$- and $B$-matrices of the
  Ising fusion system, constructs the controlled-$Z$ gates used to measure
  the $(1,2)$ and $(2,3)$ fusion channels, and computes the probabilities
  $p_{(12)}(0)$ and $p_{(23)}(0)$ symbolically. Confirms the conclusions
  stated in the thesis: the two observables are not simultaneously
  measurable, and the measurement of the first destroys information about
  the second.
- **`FibonacciFootprint.ipynb`** — the Fibonacci analogue of the above. The
  algebra is more involved because the Fibonacci $F$-matrix is non-trivial
  in a larger space, but the same conclusions hold. The notebook also
  contains the proof that the braid prefactor $B_2$ cancels in the
  computation of $p_{(23)}(0)$, so the two conventions for the $R$-matrix
  (the one used in the thesis and the one used in the literature) give the
  same physical result.
- **`Ising footprint computation.docx`** and **`Fibonacci transport.docx`** —
  written notes accompanying the notebooks. These contain the intermediate
  algebraic steps that the notebooks verify symbolically.

## Background

The footprint framework organizes quantum error correction around the local
data that an error history leaves behind: the footprint map, the syndrome
algebra, the footprint fibre, and the footprint distance. The motivating
case studies in Chapter 5 of the thesis use the Ising and Fibonacci fusion
systems as concrete examples, and the calculations in Appendix D verify that
the footprint computations reproduce the expected measurement statistics.

The central object in these computations is the fusion space of $2m$ Ising
(or Fibonacci) anyons with total trivial charge, together with the $F$- and
$R$-matrices that define the fusion system. The notebooks use SymPy to
compute the action of the controlled-$Z$ and controlled-$B_2$ gates on this
space symbolically, and to verify that the resulting probabilities match the
values reported in the thesis.

## Installation

Requires Python 3.10+.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

The notebooks use SymPy for all symbolic computation. Qiskit is used only for
visualization of the measurement circuits; no quantum simulation is
performed.

## Usage

Open either notebook in Jupyter:

```bash
jupyter notebook IsingFootprint.ipynb
jupyter notebook FibonacciFootprint.ipynb
```

Each notebook is self-contained and runs end-to-end without any external
data. The symbolic computations complete in a few seconds. Outputs are
included in the committed notebooks so that the results can be inspected
without re-running the cells.

## Repository structure

```
.
├── IsingFootprint.ipynb             # symbolic verification, Ising case
├── FibonacciFootprint.ipynb         # symbolic verification, Fibonacci case
├── Ising footprint computation.pdf # accompanying written notes (Ising)
├── Fibonacci transport.pdf         # accompanying written notes (Fibonacci)
├── requirements.txt
├── LICENSE
└── README.md
```

## Conventions

The conventions used in the notebooks match those in the thesis:

- Fusion systems are defined over the label set
  $\{\mathbf{1}, \psi, \sigma\}$ (Ising) and $\{\mathbf{1}, \tau\}$ (Fibonacci).
- The Ising $F$-matrix is
  $F_{\sigma\sigma\sigma}^\sigma = \frac{1}{\sqrt{2}}
  \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$
  and the $R$-matrix is
  $R = e^{-\pi i / 8} \mathrm{diag}(1, i)$.
- The Fibonacci $F$-matrix and $R$-matrix follow the conventions in
  Chapter 3 of the thesis, with $\omega = e^{i\pi/5}$.
- The measurement circuits follow the Hadamard-test pattern from the
  thesis: an ancilla in $\ket{0}$, a Hadamard on the ancilla, a controlled
  gate on the state register, a second Hadamard, and a computational-basis
  measurement of the ancilla.

## What the notebooks verify

### Ising footprint

- The $4$-Ising code has a $2$-dimensional fusion space
  $\mathcal{H}_4^{\mathbf{1}}$, and the observable $Z_f^{(12)}$ acts as a
  logical $Z$ on this space.
- The observable $Z_f^{(23)}$, obtained from $Z_f^{(12)}$ by an $F$-move,
  acts as a logical $X$ on the same space.
- The two observables do not commute: measuring one destroys the
  information obtained by measuring the other. Symbolically, the joint
  probability of obtaining $0$ on both ancillas is not the product of the
  individual probabilities.

### Fibonacci footprint

- The same conclusions hold in the Fibonacci fusion system, where the
  fusion space is larger and the $F$-matrix is non-trivial in a
  $2$-dimensional subspace.
- The braid prefactor $B_2 = F R F^{-1}$ that appears in the circuit for
  measuring $Z_f^{(23)}$ does not change $p_{(23)}(0)$. This is the content
  of the symbolic proof in Appendix D of the thesis, and the notebook
  reproduces the derivation line by line.

## Relation to the thesis

These notebooks are the symbolic companion to Appendix D of the thesis. The
thesis states the results as propositions with proofs; these notebooks
verify each step symbolically and provide a reproducibility artifact for the
numerical claims. The conventions, notation, and results match the thesis
exactly.

If you are reading the thesis and want to check a particular step, the
notebooks are organized so that each proposition corresponds to one or two
code cells with a markdown header naming the proposition.

## Limitations

- The notebooks assume the ideal (infinite-squeezing) versions of the Ising
  and Fibonacci fusion systems. They do not model noise or decoherence.
- Only the $4$-Ising and $6$-Ising cases are covered symbolically. The
  general $2m$-Ising chain is treated analytically in Chapter 5 of the
  thesis and is not re-derived here.
- The written notes in `.docx` form contain the intermediate steps that the
  notebooks check symbolically; they are not a substitute for the
  notebooks and vice versa.

## References

1. E. Utama. *An application of the footprint approach to quantum error
   correction in fusion systems, with a view to topological quantum
   computing.* Ph.D. thesis, University of Saskatchewan, 2027.
2. S. Rayan. *A footprint approach to quantum error correction.*
   Preprint, 2026.
3. A. Kitaev. *Fault-tolerant quantum computation by anyons.*
   Ann. Phys. 303 (2003), 2–30.
4. M. H. Freedman, A. Kitaev, M. J. Larsen, Z. Wang. *Topological quantum
   computation.* Bull. Amer. Math. Soc. 40 (2003), 31–38.

## License

MIT. See `LICENSE` for details.

## Author

Edward Utama — [github.com/Lenyan-a](https://github.com/Lenyan-a)
