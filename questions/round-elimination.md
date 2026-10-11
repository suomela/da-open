# Do finite round-elimination fixed points certify every hard input-free local problem on regular trees?

**Open question.** Does every feasible, input-free, finite-alphabet
node-edge labeling problem on fixed-degree regular trees whose randomized
LOCAL complexity is **$`\Omega(\log\log n)`$** admit a **finite nontrivial
round-elimination fixed-point relaxation**?

The precise question is the following implication, with every term defined
below:

```math
\boxed{
\begin{gathered}
\text{For every fixed integer }\Delta\ge 2\text{ and every feasible problem }P,\\[0pt]
\operatorname{Rand}_P(n)=\Omega(\log\log n)\\[2pt]
\Longrightarrow\quad
\exists\text{ a finite problem }F\text{ of the same degree }\Delta:
\quad P\to_0 F,\quad Q(F)\to_0 F,\quad T_\Delta\not\to_0 F.
\end{gathered}}
```

The alphabet and constraints of $`F`$ may depend on $`P`$ and $`\Delta`$, but must be fixed
independently of the number $`n`$ of vertices. The main cases of interest have
$`\Delta\ge 3`$, especially $`\Delta=3`$. The statement also includes paths, which are the
case $`\Delta=2`$. No upper bound on the complexity of $`P`$ is assumed.

## 1. Graphs and labeling problems

Fix $`\Delta`$ once and for all. An instance graph is a finite, connected,
undirected, simple tree in which every vertex has degree either 1 or $`\Delta`$.
Thus “regular tree” here means that every nonleaf has degree $`\Delta`$; it does
not mean that a finite tree has degree $`\Delta`$ at every vertex. There is no
distinguished root.

A **half-edge** is an incidence $`(v,e)`$, where $`v`$ is an endpoint of an edge
$`e`$. A labeling assigns one output symbol to each half-edge. The two
half-edges of the same edge can receive different symbols.

A problem is a triple

```math
P=(\Sigma_P,\mathcal N_P,\mathcal E_P),
```

where:

- $`\Sigma_P`$ is a finite output alphabet;
- $`\mathcal N_P`$ is a set of multisets of $`\Delta`$ symbols from $`\Sigma_P`$, called allowed node
  configurations;
- $`\mathcal E_P`$ is a set of multisets of two symbols from $`\Sigma_P`$, called allowed edge
  configurations.

Repeated symbols are allowed. We write $`(a_1,\ldots,a_\Delta)\in\mathcal N_P`$ or
$`(a,b)\in\mathcal E_P`$ when the corresponding multiset belongs to the constraint;
the order of the entries has no significance.

A labeling is **legal** if every degree-$`\Delta`$ vertex sees an allowed node
configuration on its incident half-edges, and every edge sees an allowed
edge configuration on its two half-edges. Leaves have no node constraint.
The edge constraint is checked on every edge, including edges incident
to leaves. Each vertex chooses the output labels of its own half-edges.

The problem is **feasible** if every tree in this graph class has at least
one legal labeling. The alphabet, both constraint sets, and $`\Delta`$ are fixed
constants, not functions of $`n`$. These finite node and edge checks are the
meaning of “locally checkable” in this question; the question uses this
presentation explicitly, rather than an unspecified encoding of a more
general local problem.

The problem is **input-free**: the task has no supplied coloring,
orientation promise, auxiliary labeling, or other task-specific input.
The structural port conventions in the next section are available, but
the output constraints cannot depend on them.

## 2. Randomized LOCAL complexity

Use the following synchronous distributed model. Initially, each vertex
knows $`n`$, $`\Delta`$, its own degree, the problem specification, and a numbering of
its incident half-edges by $`1,\ldots,\deg(v)`$. These local port numberings are
arbitrary and need not agree across an edge.

Also allow an arbitrary orientation of every edge, known to its two
endpoints; equivalently, give the two endpoints of each edge distinct
endpoint numbers 1 and 2. There is no promise about this orientation.
An algorithm must work for every choice of these structural data. In
particular, it is not given a sinkless orientation or a bipartition.

Each vertex has its own independent stream of unbiased random bits.
No shared randomness or globally unique identifiers are supplied in this
randomized convention. In each round, a vertex may send a different
message to each neighbor and then update its state from the received
messages. There is no bound on message size or local computation.

Define $`\operatorname{Rand}_P(n)`$ to be the minimum number $`t`$ of rounds sufficient for an
algorithm to output a legal $`P`$-labeling, with probability at least
$`1-1/n`$, on **every** $`n`$-vertex tree in the class and for **every**
allowed port numbering and arbitrary edge orientation. All vertices must
output by round $`t`$. The probability is over the independent private random
bits, and success means that **all** node and edge constraints hold
simultaneously. This is a global success guarantee, not a guarantee for
one vertex at a time.

The lower bound $`\operatorname{Rand}_P(n)=\Omega(\log\log n)`$ means that there are constants
$`c>0`$ and $`n_0`$ such that $`\operatorname{Rand}_P(n)\ge c\log\log n`$ for every admissible
$`n\ge n_0`$. The constants may depend on $`P`$ and $`\Delta`$. Only sizes for which the
specified trees exist are considered: for $`n\ge 2`$ these have the form
$`n=(\Delta-1)k+2`$, where $`k`$ is the number of degree-$`\Delta`$ vertices.
The base of the logarithm does not affect this condition.

## 3. The relaxation relation P →₀ F

Let $`P`$ and $`F`$ be problems of the same degree $`\Delta`$. The relation $`P\to_0 F`$ is
defined by the following finite mapping condition.

Choose an ordered representative
$`C=(c_1,\ldots,c_\Delta)`$ for each multiset $`C`$ in $`\mathcal N_P`$. An **occurrence** is a
pair $`(C,i)`$ with $`1\le i\le\Delta`$. Choose one target symbol

```math
f(C,i)\in\Sigma_F
```

for every source occurrence. The choices must satisfy both conditions:

1. For every $`C`$ in $`\mathcal N_P`$,
   $`(f(C,1),\ldots,f(C,\Delta))\in\mathcal N_F`$.
2. For every pair of occurrences $`(C,i)`$ and $`(D,j)`$, if
   $`(c_i,d_j)\in\mathcal E_P`$, then $`(f(C,i),f(D,j))\in\mathcal E_F`$.

The second condition includes $`C=D`$ and $`i=j`$: the two occurrences
can be used at different vertices even when their descriptions coincide.
All compatible occurrence pairs must be checked. The existence of such
a map does not depend on the chosen ordered representatives.

This is the **occurrence-dependent zero-round relaxation** relation used
in the question. Two occurrences of the same source symbol may have
different images, including two occurrences in the same node row. Thus
the map need not come from a single function $`\Sigma_P\to\Sigma_F`$. The mapping
conditions above, applied to the written constraints, are the definition
of the arrow; no additional map on unconstrained leaf configurations is
part of it.

## 4. The exact round-elimination operator Q

We define two finite transformations $`R`$ and $`\overline R`$, and set

```math
Q(P)=\overline R(R(P)).
```

In each transformation a new label is a **nonempty subset** of the old
alphabet. The empty subset is never a candidate label.

For a collection of valid $`k`$-tuples of such sets, a tuple
$`(S_1,\ldots,S_k)`$ is **maximal** if there is no other valid tuple
$`(S'_1,\ldots,S'_k)`$ and permutation $`\pi`$ of $`1,\ldots,k`$ such that
$`S_i\subseteq S'_{\pi(i)}`$ for every $`i`$, with at least one strict inclusion.
Tuples of sets, like configurations of ordinary labels, are considered
up to permutation.

### The transformation R

Starting from $`P`$:

1. Form all pairs $`(S,T)`$ of nonempty subsets of $`\Sigma_P`$ such that
   $`(s,t)\in\mathcal E_P`$ for **every** $`s`$ in $`S`$ and $`t`$ in $`T`$. Keep exactly the maximal
   such pairs. These are the edge configurations $`\mathcal E_{R(P)}`$.
2. The new alphabet $`\Sigma_{R(P)}`$ consists precisely of the sets that occur as
   coordinates in those retained edge configurations.
3. A tuple $`(S_1,\ldots,S_\Delta)`$ of retained labels belongs to $`\mathcal N_{R(P)}`$ if there
   **exist** choices $`s_i\in S_i`$ for which
   $`(s_1,\ldots,s_\Delta)\in\mathcal N_P`$.

### The transformation R̄

For any problem $`P`$:

1. Form all $`\Delta`$-tuples $`(S_1,\ldots,S_\Delta)`$ of nonempty subsets of $`\Sigma_P`$ such that
   $`(s_1,\ldots,s_\Delta)\in\mathcal N_P`$ for **every** choice $`s_i\in S_i`$. Keep exactly
   the maximal such tuples. These are the node configurations $`\mathcal N_{\overline R(P)}`$.
2. The new alphabet $`\Sigma_{\overline R(P)}`$ consists precisely of the sets that occur as
   coordinates in those retained node configurations.
3. A pair $`(S,T)`$ of retained labels belongs to $`\mathcal E_{\overline R(P)}`$ if there **exist**
   $`s`$ in $`S`$ and $`t`$ in $`T`$ with $`(s,t)\in\mathcal E_P`$.

These prescriptions specify the entire transformations. In particular,
maximality and the stated alphabet restrictions are part of the
definition. There is no further implicit deletion of node or edge rows.

## 5. Finite nontrivial fixed-point relaxations

Let $`T_\Delta`$ be the one-symbol problem with alphabet $`\{t\}`$, the single node
configuration containing $`\Delta`$ copies of $`t`$, and the single edge configuration
$`(t,t)`$.

A problem $`F`$ is **nontrivial** for the certificate condition if
$`T_\Delta\not\to_0 F`$, meaning that the relaxation map in Section 3 does not exist.
Equivalently, there is no allowed node configuration
$`(b_1,\ldots,b_\Delta)\in\mathcal N_F`$ for which

```math
(b_i,b_j)\in\mathcal E_F
\qquad\text{for every }i,j\in\{1,\ldots,\Delta\}.
```

The pairs with $`i=j`$ are included, so all required self-pairs must be
allowed as well. This is an explicit finite test for the nontriviality
condition in the question.

A **fixed point** here means a problem $`F`$ satisfying
$`Q(F)\to_0 F`$. This is the relaxed notion: literal equality of $`F`$ and $`Q(F)`$,
even up to renaming labels, is not required.

A **finite nontrivial fixed-point relaxation of $`P`$** is a problem $`F`$ with
a finite alphabet satisfying all three conditions

```math
P\to_0 F,\qquad Q(F)\to_0 F,\qquad T_\Delta\not\to_0 F.
```

Both arrows allow the full occurrence-dependent maps defined above.
$`F`$ is input-free and has the same degree as $`P`$. There is no prescribed
bound on its alphabet size, and its labels need not be subsets of $`\Sigma_P`$.

## 6. What would solve this question

A **positive solution** is a proof of the boxed implication for every
fixed $`\Delta`$ and every feasible finite $`P`$ in the stated class. An existence
proof suffices; a particular constructor or an explicit bound on the size
of $`F`$ is not required.

A **negative solution** is one fixed degree $`\Delta`$ and one explicitly
specified finite, feasible, input-free problem $`P`$, together with proofs
that:

1. $`\operatorname{Rand}_P(n)=\Omega(\log\log n)`$ in the model above; and
2. for every finite problem $`F`$ of degree $`\Delta`$,
   $`P\to_0 F`$ and $`Q(F)\to_0 F`$ imply $`T_\Delta\to_0 F`$.

A counterexample at $`\Delta=3`$ would suffice. The second requirement must
cover arbitrarily large finite alphabets and all occurrence-dependent
maps. A search excluding only finitely many target sizes, a construction
of an infinite target, or a counterexample that requires a promised
auxiliary input does not settle this question.

## Reference and scope

The terminology and round-elimination conventions follow Alkida Balliu,
Sebastian Brandt, Ole Gabsdil, Dennis Olivetti, and Jukka Suomela,
*On the Universality of Round Elimination Fixed Points*,
[arXiv:2510.17639v2](https://arxiv.org/abs/2510.17639v2), especially
Definitions 2.2, 2.4, 2.9, and 2.13. That paper gives a negative result
when a promised input labeling is available; the question here concerns
the input-free setting defined above. All definitions needed to state
and solve this question are contained in this file.
