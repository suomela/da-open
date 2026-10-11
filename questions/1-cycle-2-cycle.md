# Open question: one undirected cycle versus two equal cycles in MPC

**Prove at least one of the following for the precise problem and model
defined below:**

- An unconditional **superconstant round lower bound** with linear total
  memory and local memory proportional to \(n^\delta\) words, for at least
  one fixed constant \(0<\delta<1\).
- An **\(o(\log n)\)-round algorithm** with polynomial total memory and
  local memory proportional to \(n^\delta\) words, for at least one fixed
  constant \(0<\delta<1\).

Either outcome would meet the research objective. They are not mutually
exclusive. A logarithmic lower bound would be stronger than required;
any lower bound tending to infinity would suffice. The definitions and
success criteria in this file require no accompanying research notes.

**The input and answer.** Let \(n\ge 6\) be even, and write
\([n]=\{1,\ldots,n\}\). The input is a simple undirected graph
\(G=([n],E)\), promised to have exactly one of these two forms:

\[
G\cong C_n
\qquad\text{or}\qquad
G\cong C_{n/2}\sqcup C_{n/2}.
\]

Here \(C_k\) is a simple cycle on \(k\) vertices, \(\cong\) means graph
isomorphism, and \(\sqcup\) means disjoint union. Thus every vertex has
degree two, there are exactly \(n\) edges, and in the disconnected case
the two components each have **exactly \(n/2\) vertices**. Labels are
arbitrary: their numerical order carries no information about cyclic
order or component membership.

The input consists of one record \(\{u,v\}\) for each edge, distributed
among the machines in an arbitrary order and an arbitrary partition
subject to their memory capacities. Each edge record takes a constant
number of words, so the input size is \(\Theta(n)\) words. Initially
each edge is supplied once. All machines know \(n\), the promise, and
the public machine identifiers. They receive no consistent edge
orientation, successor permutation, traversal order, component labels,
geometric embedding, or other input-dependent auxiliary information.
Any preprocessing to obtain such information must be performed within
the model and charged to the algorithm.

A designated machine, fixed before the input is chosen, must output
one bit: \(1\) for \(C_n\) and \(0\) for two equal cycles. It need not
output a traversal, component labels, or any other certificate.

**The computational model.** MPC means *massively parallel computation*,
with classical machines and classical messages. Use the following
ordinary synchronous model:

1. **Local memory and words.** There are \(M=M(n)\) physical machines,
   each with capacity \(S=S(n)=\Theta(n^\delta)\) words. A word has
   \(\Theta(\log n)\) bits, with fixed constants sufficient to encode
   vertex and machine identifiers. All local input, retained data,
   message buffers, and working storage count toward capacity, up to
   fixed constant-factor workspace allowances. Neither an arbitrarily
   long integer nor an exact real number is a single word.

2. **Total memory.** The aggregate memory is the allocated capacity
   \(T=MS\), including idle machines and helpers. Linear total memory
   means \(T=O(n)\) words; polynomial total memory means
   \(T\le C n^K\) words for fixed constants \(C,K\). Replicated data
   and all intermediate storage count. Virtual processes do not each
   acquire a separate free memory allowance.

3. **Local computation.** Between communications, each machine may
   perform arbitrary local computation on its available information.
   There is no local time bound and no restriction to linear,
   algebraic, comparison-based, or edge-preserving operations. Local
   transition functions may depend on \(n\) and machine identity and
   are fixed independently of the input. In particular, a lower bound
   must allow arbitrary such functions, including nonuniform choices
   for different input sizes. Input-dependent information becomes
   available only through the machine's input and received messages.

4. **One communication round.** Based on its current local information,
   each machine selects messages and their destination machines. It
   sends at most \(O(S)\) words in total and receives at most \(O(S)\)
   words in total. Communication is all-to-all: a destination need not
   be a graph neighbor. Destinations, message contents, message
   lengths, and the decision to send nothing may depend on the data.
   Delivered messages become available after this simultaneous
   communication step. A new outgoing message that depends on them
   must wait for a later round. Local computation after the last
   communication is allowed without another round.

5. **Communication accounting.** Addressing and message framing use
   \(O(\log n)\)-bit identifiers and are charged within the word
   budgets. Every transmitted copy is charged to its sender and
   receiver. There is no free broadcast, global aggregation, sorting,
   shared input store, or remote-memory oracle. A receiver may use the
   identities of senders and the absence of messages in its local
   computation; a lower bound cannot assume fixed routing or ignore
   information conveyed by silence. Sending a notification counts as
   communication even if it has no data payload.

6. **Randomness and error.** Public and private random coins are
   allowed and are independent of the input. For **every** promised
   labeled graph and **every** legal initial partition, the designated
   output must be correct with probability at least \(2/3\), over the
   algorithm's coins. A deterministic correct algorithm also
   qualifies. Success only on average over a chosen input distribution
   is insufficient for an upper bound.

7. **Resource convention.** For definiteness, use hard capacity bounds
   and a worst-case bound on the number of rounds: they must hold on
   every execution on a promised input. An algorithm may return a
   failure symbol, counted as an incorrect answer, but cannot exceed
   its budgets on that execution. No correctness or resource behavior
   is required on graphs outside the promise. All initialization,
   input-dependent preprocessing, communication, and final output
   collection must be included in the resource analysis.

This is ordinary MPC: several dependent remote lookups cannot be
performed inside one round merely by calling them adaptive queries.

**Precisely what would count as a solution.** Establish either A or B.
All asymptotics below are as \(n\) tends to infinity through even
integers. Constants in word size and memory or communication allowances
are independent of \(n\).

**A. A growing lower bound.** Exhibit one constant \(\delta\in(0,1)\)
for which the bounded-error round complexity in the model above, with
\(S=\Theta(n^\delta)\) and \(T=O(n)\), is \(\omega(1)\).
Here \(\omega(1)\) means a lower bound tending to infinity, however
slowly. The lower bound must hold against all randomized protocols
permitted by the model, with no restriction on their message encodings
or local computations.

More explicitly: for that **same \(\delta\)**, for every fixed round
count \(r\) and every fixed choice of the constants in the local,
aggregate, word-size, and communication budgets, prove that there is
\(n_0\) such that, at every even \(n\ge n_0\), every protocol using
at most \(r\) rounds within those budgets has some promised graph and
legal initial partition on which its success probability is less than
\(2/3\). The threshold \(n_0\) may depend on \(r\) and those
constants. This includes every fixed constant amount of
linear-memory workspace; it is not a claim only for an allocation with
no spare space. A lower bound valid with polynomial aggregate memory
would also qualify and would be stronger than necessary.

**B. A sublogarithmic algorithm.** Exhibit constants
\(\delta\in(0,1)\) and finite \(K\), independent of \(n\), and a
fully specified ordinary-MPC algorithm for all sufficiently large even
\(n\) such that

\[
S=O(n^\delta)\ \text{words},\qquad
T=O(n^K)\ \text{words},\qquad
r(n)=o(\log n).
\]

The last condition means \(r(n)/\log n\to0\). Prove its worst-case
success probability is at least \(2/3\), and give its complete memory,
send-volume, receive-volume, and round accounting, starting from the
undirected edge input above. The exponent \(K\) may be any fixed
constant; it cannot grow with \(n\). Linear aggregate memory would be
an additional improvement, not a requirement for this outcome.

For either outcome, **one fixed local-memory exponent is enough**, and
the exponents used for A and B need not be the same. The parameter
\(n\) always refers to the original input's vertex count, including
when an argument constructs larger auxiliary instances.

The requested result must be unconditional. A theorem assuming another
unproved hardness conjecture, or reducing this task to an unresolved
auxiliary problem, does not by itself settle A or B.

**Boundaries of the objective.** Excluding two rounds, three rounds,
or any other single fixed round count does not establish A. Neither
does a lower bound proportional to \(\log n/\log S\), since this is
constant when \(S=n^\delta\). Proving a bound for each \(r\) while
choosing a smaller \(\delta=\delta(r)\) each time also does not establish
A: the same positive \(\delta\) must work for arbitrarily large fixed
\(r\).

For B, a constant-factor improvement to an \(O(\log n)\) round count
is insufficient. An algorithm requiring
\(S=n/\operatorname{polylog}n=n^{1-o(1)}\) does not meet the fixed
\(\delta<1\) condition. An algorithm starting with a supplied
orientation, cyclic order, or component decomposition must first show
how to obtain that information within the stated bounds.

A lower bound restricted to forwarding edges, explicitly storing
paths, using linear summaries, or processing each graph component
independently does not cover the allowed protocols. An algebraic
identity, finite experiment, or result for a different computation
model counts toward the objective only when accompanied by a complete
proof transferring it to A or B for this exact input promise and model.
