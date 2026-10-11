# Does fast distributed matrix multiplication force fast centralized matrix multiplication?

We want to understand whether the communication complexity of **ordinary dense square matrix multiplication in standard distributed models** is determined by its centralized arithmetic complexity. The distributed models allow **unrestricted local computation**. The principal open target is a converse to the known reductions from centralized arithmetic algorithms to distributed communication algorithms.

**The main question is:** does the existence of arbitrarily small round exponents for matrix multiplication in the standard congested clique imply that the centralized matrix-multiplication exponent is two?

The definitions below make this question precise. A resolution of this main question would meet our objective. Stronger quantitative versions and an alternative algorithmic success criterion are stated separately.

**Field and computational task.**

Fix a finite field $`F=\mathbb F_q`$, with $`q`$ independent of the matrix dimension. Every centralized and distributed quantity in a comparison refers to this **same field**. The concrete case $`F=\mathbb F_2`$ is already a satisfactory target; a result over arbitrary fixed finite fields would be stronger. Over $`\mathbb F_2`$, multiplication means ordinary field matrix multiplication, with addition modulo two, not the Boolean OR–AND product.

For each positive integer $`n`$, there are exactly $`n`$ computers, with public identifiers $`1,\ldots,n`$, and two arbitrary matrices $`A,B\in F^{n\times n}`$. Computer $`i`$ initially knows row $`i`$ of both $`A`$ and $`B`$. It must eventually output row $`i`$ of the exact product $`C=AB`$, where

```math
C_{ij}=\sum_{k=1}^n A_{ik}B_{kj}.
```

There is no sparsity, rank, or other promise on the inputs. “Dense” means that the algorithm must handle every pair of matrices. All input redistribution, intermediate communication, and delivery of the specified output rows are charged.

**Distributed models.**

Communication takes place in synchronous rounds on a complete network. A word has $`O(\log(n+2))`$ bits, with a constant independent of $`n`$. Messages may be arbitrary bit strings of that length; they need not be field elements or algebraic expressions. Node identifiers and routing metadata must fit within the stated communication budgets.

| Model | Communication in one round | Local memory |
|---|---|---|
| Unicast congested clique, abbreviated CC | Each ordered pair of distinct computers can exchange $`O(1)`$ words. Different neighbors may receive different messages. | Unrestricted. |
| Low-bandwidth clique, abbreviated LB | Each computer sends at most one word and receives at most one word in total, to/from arbitrary computers. Algorithms must respect the receive bound as well as the send bound. | Unrestricted. |
| Massively parallel computation, abbreviated MPC | Each computer sends and receives $`O(n)`$ words in total, to/from arbitrary computers. | $`O(n)`$ words per computer, including working state and communication buffers; $`O(n^2)`$ total memory. Constant factors may accommodate the input and output rows. |

All three models permit **arbitrarily much local computation between communication rounds**, subject only to the MPC memory bound where applicable. Local computation may branch, use bit operations, and compute arbitrary computable functions of the information available locally. There is no assumption that messages are linear or bilinear, that local computation follows a prescribed arithmetic circuit, or that work is balanced between computers.

Algorithms have finite, uniformly specified descriptions, may know $`n`$, and must terminate. There is no local running-time bound. Constants may depend on the algorithm and the fixed field but not on $`n`$ or the input. There is no input-dependent advice or preprocessing supplied for free.

The initial question concerns **deterministic algorithms that are correct on every input**, with worst-case round complexity. A randomized result must state its error guarantee and is a separately identified extension, not silently substituted for this question.

The usual node-capacitated clique allows a polylogarithmic number of words per node per round. For the polynomial-exponent questions here, its fixed polylogarithmic bandwidth allowance can be absorbed into arbitrarily small exponent slack. LB above fixes a precise one-word convention.

**Centralized arithmetic complexity.**

Let $`C_F(n)`$ be the minimum number of scalar additions, subtractions, and multiplications over $`F`$ in a division-free arithmetic circuit that computes all $`n^2`$ entries of the formal matrix product. Constants from $`F`$ are allowed. Correctness means equality as polynomials in the $`2n^2`$ input indeterminates, not merely equality of functions after imposing identities such as $`x^q=x`$.

Define the standard matrix-multiplication exponent by

```math
\omega_F=\inf\{\tau:C_F(n)=O(n^\tau)\}.
```

Thus $`\omega_F=2`$ means that, for every $`\delta>0`$, the formal product has arithmetic complexity $`O_\delta(n^{2+\delta})`$. It does not assert an $`O(n^2)`$ algorithm with no slack. This is the usual arithmetic exponent, not the running time of a centralized machine given arbitrary lookup operations at unit cost.

**Distributed exponents and quantifiers.**

For a model $`X`$, let $`T_{\mathcal A,X}(n)`$ be the worst-case number of rounds used by an allowed algorithm $`\mathcal A`$ for the task above. Define

```math
\alpha_{X,F}
=\inf\{r\ge0:\text{there exists such an algorithm }\mathcal A
\text{ with }T_{\mathcal A,X}(n)=O(n^r)\}.
```

Write $`\rho_F=\alpha_{\mathrm{CC},F}`$, $`\lambda_F=\alpha_{\mathrm{LB},F}`$, and $`\mu_F=\alpha_{\mathrm{MPC},F}`$. The algorithm and the constant in the $`O(\cdot)`$ bound may depend on the chosen exponent. Bounds must hold for all sufficiently large $`n`$, not just selected dimensions.

In particular, $`\rho_F=0`$ means:

> For every $`\varepsilon>0`$, there exists a deterministic CC algorithm $`\mathcal A_\varepsilon`$ solving the specified task on every input, for all sufficiently large $`n`$, in $`O_\varepsilon(n^\varepsilon)`$ rounds.

This is an infimum-of-exponents statement. It does not require one fixed algorithm with $`n^{o(1)}`$ rounds, and does not mean constant or polylogarithmic rounds.

**Known direction.**

Standard constructions give

```math
\rho_F\le 1-\frac{2}{\omega_F},\qquad
\lambda_F\le 2-\frac{2}{\omega_F},\qquad
\mu_F\le \frac{\omega_F}{2}-1.
```

Each bound is interpreted with arbitrary positive exponent slack. The MPC bound includes the stated $`O(n)`$ local memory constraint. These are upper bounds, not established equalities. Also $`\omega_F\ge2`$, and the elementary information bound for the specified LB task gives $`\lambda_F\ge1`$; logarithmic factors from word size do not affect these exponents.

Consequently, $`\omega_F=2`$ already implies $`\rho_F=0`$, $`\lambda_F=1`$, and $`\mu_F=0`$.

**Principal open question.**

Prove or refute the converse

```math
\boxed{\rho_F=0\quad\Longrightarrow\quad\omega_F=2.}
```

Equivalently, is $`\rho_F=0`$ if and only if $`\omega_F=2`$, in the models and with the quantifiers above?

A proof must cover every allowed family of CC algorithms witnessing the premise, regardless of how expensive or nonalgebraic their local computations are. It need not obtain a fast centralized algorithm by directly simulating those computations. Nor must a procedure extracting an arithmetic algorithm from a distributed protocol itself be efficient; an existence proof of the required arithmetic algorithms is sufficient.

A refutation must actually establish that the implication is false for the same specified field. Showing that a particular simulation or proof strategy fails is not a refutation.

**Stronger quantitative questions and the other models.**

The natural quantitative converse targets are:

| Premise about distributed complexity | Desired centralized conclusion |
|---|---|
| $`\rho_F\le r`$, where $`0\le r<1`$ | $`\omega_F\le 2/(1-r)`$ |
| $`\lambda_F\le1+d`$, where $`0\le d<1`$ | $`\omega_F\le 2/(1-d)`$ |
| $`\mu_F\le r`$, where $`r\ge0`$ | $`\omega_F\le2+2r`$ |

Together with the known direction, these would give, respectively,

```math
\rho_F=1-\frac2{\omega_F},\qquad
\lambda_F=2-\frac2{\omega_F},\qquad
\mu_F=\frac{\omega_F}{2}-1.
```

Even the endpoint converses $`\lambda_F=1\Rightarrow\omega_F=2`$ or $`\mu_F=0\Rightarrow\omega_F=2`$ would be satisfactory resolutions of the corresponding research target. Resolving one model is valuable on its own; solving all three is not required. These converse statements are questions, not assumptions available in a proof.

**Alternative success: a faster distributed algorithm.**

If the converse remains out of reach, another concrete objective is a better algorithm for the same dense task in any one of these standard models. The $`\omega_F\le9/4`$ bound yields current arithmetic-driven benchmark exponents

```math
\rho_F\le1/9,\qquad \lambda_F\le10/9,\qquad \mu_F\le1/8,
```

again with arbitrary positive slack. These numerical benchmarks are recorded as of 11 October 2026.

A satisfactory algorithmic result is an explicit construction with worst-case round complexity, for some fixed $`\eta>0`$, of at least one of

```math
O(n^{1/9-\eta})\ \text{in CC},\qquad
O(n^{10/9-\eta})\ \text{in LB},\qquad
O(n^{1/8-\eta})\ \text{in the stated MPC regime}.
```

The exponent gap must be independent of $`n`$, and the algorithm must handle all inputs and all sufficiently large dimensions. Polylogarithmic factors can be absorbed by decreasing a strict positive gap. Encoding, routing, decoding, and MPC memory must all be accounted for. Beating these numerical benchmarks would not by itself refute the proposed equalities: the true value of $`\omega_F`$ might be smaller than its current upper bound.

**What would not answer this question.**

- Restricting local computation time, local arithmetic rank, recomputation, or the forms of messages before proving a converse or lower bound.
- Proving optimal communication only for a specified arithmetic algorithm, tensor decomposition, or computation graph and treating it as a bound against all protocols.
- Adding computers, increasing the MPC memory beyond $`O(n)`$ words per computer, using unbounded-precision messages, or supplying input-dependent preprocessing without charging for it.
- Replacing exact dense multiplication by verification, a promise decision problem, a sparse special case, a different matrix product, or a different input/output distribution without paying its conversion cost.
- Inferring a fast arithmetic algorithm just by counting communicated words. Those words may have required arbitrarily expensive local computation.

Using a restricted family to **construct an upper-bound algorithm** is entirely legitimate: the resulting protocol need only meet the original task and model requirements. What is not legitimate is restricting the competing protocols when asserting an unrestricted converse.

**Source pointers, not prerequisites for understanding the question.**

The forward constructions are documented in [Censor-Hillel et al., *Algebraic Methods in the Congested Clique*](https://jukkasuomela.fi/doc/algebraic-clique.pdf), [Ballard et al., *Communication-Optimal Parallel Algorithm for Strassen's Matrix Multiplication*](https://arxiv.org/abs/1202.3173), and [Gupta–Suomela–Vahidi, *Rectangular Matrix Multiplication in the Low-Bandwidth Model*](https://arxiv.org/abs/2606.04652). The numerical benchmark uses the [October 2026 $`9/4`$ preprint](https://github.com/openai/math/blob/fd4aeeb2ee4fc729c18d98444fed42fd0529eeeb/preprints/Matrix-Multiplication-Nine-Fourths-October-2-2026/paper.pdf) and the [all-fields extension](https://github.com/jzuiddam/omega-nine-quarters-all-fields). The principal converse question does not depend on the current numerical upper bound.
