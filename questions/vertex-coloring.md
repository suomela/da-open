# The degree dependence of deterministic LOCAL vertex coloring

We study the round complexity of **proper vertex coloring on arbitrary
graphs in the deterministic LOCAL model**. Here $`n`$ is the number of
vertices and $`\Delta`$ is the maximum degree. The primary target is
$`(\Delta+1)`$-coloring with running time
$`f(\Delta)+O(\log^* n)`$. We also accept the two specific advances below
on the number of colors achievable in $`O(\log^* n)`$ rounds.

We seek a resolution of **at least one** of the following four questions:

1. **Time upper bound for $`(\Delta+1)`$-coloring:** Can we give an algorithm taking
   $`f(\Delta)+O(\log^* n)`$ rounds with
   $`f(\Delta)=O(\Delta^{1/2-\varepsilon})`$ for some fixed constant
   $`\varepsilon>0`$ independent of $`n`$ and $`\Delta`$?
2. **Palette upper bound in $`O(\log^* n)`$ rounds:** Can we compute a
   proper $`O(\Delta^{2-\varepsilon})`$-vertex coloring in
   $`O(\log^* n)`$ rounds for some fixed constant $`\varepsilon>0`$?
3. **Time lower bound for $`(\Delta+1)`$-coloring:** Can we prove that a
   uniform $`O(\log^* n)`$-round algorithm is impossible, so that some
   additional, unbounded dependence on $`\Delta`$ is necessary?
4. **Palette lower bound in $`O(\log^* n)`$ rounds:** Can we prove that
   computing a proper $`O(\Delta^{1+\varepsilon})`$-vertex coloring in
   $`O(\log^* n)`$ rounds is impossible for some fixed constant
   $`\varepsilon>0`$?

A complete solution to any one of these questions is a successful outcome.
Determining the optimal complexity, or resolving all four questions, is
not required. The constants $`\varepsilon`$ in different questions need
not be the same; each must be independent of $`n`$ and $`\Delta`$.

**The graph problem.** The input is any finite simple undirected graph
$`G=(V,E)`$, including disconnected graphs. Write $`n=|V|\ge1`$ and
$`\Delta=\max_{v\in V}\deg(v)`$. Given the palette bound $`q(\Delta)`$
specified by the question, each vertex must output a color from the common
palette $`\{1,\ldots,q(\Delta)\}`$, and the outputs must satisfy

```math
\varphi(u)\ne\varphi(v)\qquad\text{for every edge }\{u,v\}\in E.
```

Using fewer than $`q(\Delta)`$ colors is allowed. Questions 1 and 3 use
$`q(\Delta)=\Delta+1`$; Questions 2 and 4 specify different palette bounds.
There are no assumptions about arboricity, girth, neighborhood
independence, or any other graph structure.

**The computational model.** There is one processor at each vertex.
Communication proceeds in synchronous rounds along the edges of $`G`$.
In each round, a vertex may send a separate, arbitrarily large message to
each neighbor, receive its neighbors' messages, and perform arbitrary
finite local computation. Only communication rounds are charged.

Vertices have distinct identifiers from a polynomial-size namespace:
identifiers are integers in $`\{1,\ldots,\lceil n^c\rceil\}`$, for a fixed
constant $`c\ge1`$. The exponent is fixed independently of $`n`$ and
$`\Delta`$; complexity constants may depend on it. The algorithm must work
for every legal identifier assignment. Each vertex can distinguish its
incident edges by arbitrary local port numbers; no consistency between
different vertices' port orders is assumed.

Initially, a vertex knows its own identifier, its incident ports and
degree, and the global values $`n`$ and $`\Delta`$. Neighbors' identifiers
can be learned in one round. No proper coloring, edge orientation,
decomposition, or other auxiliary labeling is supplied. The algorithm may
construct and use any such information, with all communication counted.
It may use identifiers, ports, and the full information obtained through
communication. It is not required to be anonymous, comparison-based,
locally iterative, or restricted to maintaining a coloring as its state.

The algorithm must be deterministic and always correct. A single finite
program must handle all input sizes and degrees; there is no advice
depending on the particular graph. Randomness, quantum communication, and
communication between nonadjacent vertices are outside this model.

**The required asymptotic form.** Let $`T_A(n,\Delta)`$ be the worst-case
number of rounds used by an algorithm $`A`$ that always computes a proper
coloring within its stated palette bound, over all $`n`$-vertex graphs of
maximum degree exactly $`\Delta`$, all legal identifiers, and all port
numberings. Let $`\log^* n`$ denote the number of iterations of the
base-two logarithm needed to obtain a value at most $`1`$.

An upper bound of the desired form means that there are a function
$`f:\mathbb N_0\to\mathbb R_{\ge0}`$ and constants $`C>0`$, $`B\ge0`$ such
that

```math
T_A(n,\Delta)\le f(\Delta)+C\log^* n+B
\qquad\text{for every }n\ge1\text{ and }0\le\Delta\le n-1.
```

The function $`f`$ is independent of $`n`$, and $`C,B`$ are independent of
both $`n`$ and $`\Delta`$. The bound must cover every relationship between
these parameters. Treating $`\Delta`$ as a fixed constant and allowing the
coefficient of $`\log^* n`$ to depend on it does not establish this bound.
In Questions 2 and 4, the time bound is specifically
$`C\log^* n+B`$, with no additional unbounded degree-dependent term.

**Question 1: time upper bound, precisely.** Construct and prove correct a deterministic
LOCAL $`(\Delta+1)`$-coloring algorithm $`A`$ for which there exist constants
$`0<\varepsilon<1/2`$, $`K>0`$, $`C>0`$, and $`B\ge0`$ such that

```math
T_A(n,\Delta)\le K\Delta^{1/2-\varepsilon}+C\log^* n+B
\qquad\text{for every }n\ge1\text{ and }0\le\Delta\le n-1.
```

All four constants are independent of $`n`$ and $`\Delta`$. It suffices
to achieve this for one fixed positive $`\varepsilon`$, however small;
an algorithm for every choice of $`\varepsilon`$ is not required.

The target is a **fixed polynomial improvement** below $`\sqrt\Delta`$.
For example, $`f(\Delta)=O(\Delta^{0.499})`$ would suffice. A bound of
$`O(\sqrt\Delta/(\log\Delta)^k)`$, for any fixed $`k>0`$, does not
qualify. Merely proving $`f(\Delta)=o(\sqrt\Delta)`$ is insufficient,
and an exponent gain $`\varepsilon(\Delta)`$ tending to zero is also
insufficient unless the bound can separately be strengthened to a fixed
positive exponent gain. An $`O(\log^* n)`$-round algorithm would answer
Question 1 in a stronger form and give a negative answer to Question 3.

**Question 2: palette upper bound, precisely.** Construct a deterministic LOCAL algorithm $`A`$
and constants $`0<\varepsilon<1`$, $`K>0`$, $`C>0`$, and $`B\ge0`$ such
that, on every input graph, $`A`$ computes a proper coloring from the
common palette

```math
\{1,\ldots,\lceil K D^{2-\varepsilon}\rceil\},
\qquad D=\max\{1,\Delta\},
```

in at most $`C\log^* n+B`$ rounds. All constants are independent of
$`n`$ and $`\Delta`$. The use of $`D`$ only ensures that the palette is
nonempty when $`\Delta=0`$. Restricting $`\varepsilon`$ to $`(0,1)`$
loses no qualifying stronger upper bound.

A fixed polynomial reduction in the palette exponent is required:
$`O(\Delta^{1.999})`$ colors would suffice, whereas
$`O(\Delta^2/(\log\Delta)^k)`$ colors for a fixed $`k>0`$ would not.
This result is sufficient progress on its own; reducing the output
further to $`\Delta+1`$ colors is not required for Question 2.

**Question 3: time lower bound, precisely.** Prove that there is no correct
deterministic LOCAL $`(\Delta+1)`$-coloring algorithm with a uniform
$`O(\log^* n)`$ round bound. Equivalently, prove, with $`A`$ ranging over
algorithms that always return a proper $`(\Delta+1)`$-coloring,

```math
\forall A\text{ correct}\;\forall C>0\;\forall B\ge0\;
\exists n,\Delta:\qquad
T_A(n,\Delta)>C\log^* n+B.
```

This is the operational target behind asking whether an additional
$`f(\Delta)=\omega(1)`$ term is necessary: exclude every bounded choice of
$`f`$ in the uniform additive bound. A quantitative lower bound explaining
how the necessary cost grows with $`\Delta`$ would be stronger progress.

The decomposition into $`f(\Delta)`$ and $`O(\log^* n)`$ is not unique.
Consequently, merely proving an absolute lower bound that grows with
$`\Delta`$ is insufficient. For example, $`\Omega(\log^*\Delta)`$ rounds
does not rule out $`O(\log^* n)`$, because $`\Delta<n`$. A successful lower
bound must exclude every constant coefficient in the allowed
$`O(\log^* n)`$ term. A lower bound on a suitable hard subclass of graphs
is sufficient, provided it applies to unrestricted algorithms in the
model specified above.

**Question 4: palette lower bound, precisely.** Prove that there exists a constant
$`0<\varepsilon<1`$ such that, for every constant $`K>0`$, no
deterministic LOCAL algorithm that always computes a proper coloring from
the common palette

```math
\{1,\ldots,\lceil K D^{1+\varepsilon}\rceil\},
\qquad D=\max\{1,\Delta\},
```

has a uniform $`O(\log^* n)`$ round bound. More explicitly, for that
$`\varepsilon`$, every such $`K`$ and algorithm $`A`$, and every pair of
constants $`C>0`$, $`B\ge0`$, there must be valid $`n,\Delta`$ for which

```math
T_A(n,\Delta)>C\log^* n+B.
```

The exponent $`\varepsilon`$ is chosen once, before quantifying over
algorithms and constants. Ruling out just one fixed palette multiplier
$`K`$, or just one fixed time multiplier $`C`$, is insufficient. The
range $`0<\varepsilon<1`$ is necessary here because
$`O(\Delta^2)`$-coloring in $`O(\log^* n)`$ rounds is already known.
Question 4 would also answer Question 3 positively, since a
$`(\Delta+1)`$-coloring uses $`O(\Delta^{1+\varepsilon})`$ colors.

**What must be preserved in a solution.** Intermediate techniques may be
drawn from anywhere, including list coloring, defective coloring,
bounded-outdegree orientations, or round elimination. Their complete
application must yield at least one of the four theorems above, with its
specified palette and running time, in the stated model.

In particular, the following results by themselves do not resolve any of
the four questions:

- An edge-coloring result without a reduction to one of the stated
  vertex-coloring targets.
- An upper bound requiring a special graph class, a supplied orientation,
  or a restricted relationship between $`n`$ and $`\Delta`$.
- An upper bound with a larger dependence on $`n`$, such as an additive
  $`\mathrm{polylog}\,n`$, or an analysis with degree-dependent
  constants hidden in $`O(\log^* n)`$.
- A lower bound only for anonymous algorithms, SET-LOCAL, one-round
  algorithms, a particular color-reduction architecture, or a prescribed
  preprocessing followed by restricted postprocessing.
- Finite computational evidence without an asymptotic theorem.

For example, an algorithm on a supplied orientation of outdegree $`d`$
with lists of size $`d(d+1)`$ is a potentially useful ingredient. Applying
it here requires constructing the relevant instances, accounting for all
rounds as $`d`$ grows, and establishing the palette and time bounds of the
particular question being answered on every input graph.

A constant-round reduction from $`\Theta(\Delta^2)`$ to
$`O(\Delta^{2-\varepsilon})`$ colors is a sufficient route to Question 2
if it composes with a uniform $`O(\log^* n)`$-round preprocessing algorithm
and works on every input graph. Such a factorization is not required of
all algorithms. A lower bound restricted to this preprocessing and
postprocessing route does not by itself establish Question 3 or 4.

**Known context, checked October 11, 2026.** All four targets remain open
in the sources checked. The best known general deterministic upper bound
for $`(\Delta+1)`$-coloring in the desired additive regime is
$`O(\sqrt{\Delta\log\Delta}+\log^* n)`$. The classical
$`\Omega(\log^* n)`$ lower bound already holds on cycles and accounts for
the allowed dependence on $`n`$; it does not answer Question 3.
An $`O(\Delta^2)`$-coloring can be computed deterministically in
$`O(\log^* n)`$ rounds. The possibilities of
$`O(\Delta^{1.999})`$-coloring and $`O(\Delta^{1.001})`$-coloring in that
time are still explicitly listed as open. See
[Jukka Suomela's open-problems page](https://jukkasuomela.fi/open/) and
[Maus and Tonoyan, *Local Conflict Coloring Revisited: Linial for Lists*](https://arxiv.org/abs/2007.15251).
These references provide background; the problem, model, and success
criteria are fully specified above.
