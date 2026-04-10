# Impossibility of Robust $(n; n{-}1)$ Black-Box VRF-Combiners

- Feasibility score: 5
- Nicheness score: 9
- Publishability score: 7

## Notations and definitions

**Security parameter and asymptotics.** Let $\lambda \in \mathbb{N}$ denote the security parameter. Unless stated otherwise, all asymptotic statements are as $\lambda \to \infty$. We write $\mathsf{poly}$ for the set of functions polynomial in $\lambda$ and $\mathsf{negl}$ for the set of negligible functions. An algorithm is *efficient* (ppt.) if it is a probabilistic Turing machine running in time $\mathsf{poly}(\lambda)$.

**Verifiable function.** A verifiable function (VF) is a triple of ppt. algorithms $\mathsf{VF} = (\mathsf{Gen}, \mathsf{Eval}, \mathsf{Vfy})$ with syntax
- $\mathsf{Gen}(1^\lambda) \to (\mathsf{sk}, \mathsf{pk}) \in \mathcal{SK} \times \mathcal{PK}$,
- $\mathsf{Eval}(\mathsf{sk}, x) \to (y, \pi) \in \mathcal{Y} \times \mathcal{P}$ for $x \in \{0,1\}^\lambda$,
- $\mathsf{Vfy}(\mathsf{pk}, x, y, \pi) \to b \in \{0,1\}$.

**Security properties of a VF.** For a VF $\mathsf{VF}$:
- **Perfect correctness.** For all $\lambda$, all $(\mathsf{sk}, \mathsf{pk}) \in \mathrm{supp}(\mathsf{Gen}(1^\lambda))$ and all $x \in \{0,1\}^\lambda$, $\Pr[\mathsf{Vfy}(\mathsf{pk}, x, y, \pi) = 1 \mid (y, \pi) \gets \mathsf{Eval}(\mathsf{sk}, x)] = 1$.
- **Unique provability.** For all $\mathsf{pk} \in \mathcal{PK}$, all $x \in \{0,1\}^\lambda$ and all image-proof pairs $(y_0, \pi_0), (y_1, \pi_1)$,
$$\mathsf{Vfy}(\mathsf{pk}, x, y_0, \pi_0) = \mathsf{Vfy}(\mathsf{pk}, x, y_1, \pi_1) = 1 \;\Longrightarrow\; y_0 = y_1.$$
- **Pseudorandomness.** For all ppt. adversaries $\mathcal{A} = (\mathcal{A}_1, \mathcal{A}_2)$, the advantage
$$\mathsf{Adv}^{\mathrm{prf}}_{\mathsf{VF}}(\lambda, \mathcal{A}) := 2 \cdot \Bigl|\Pr[\mathsf{Exp}^{\mathrm{prf}}_{\mathsf{VF}}(\lambda, \mathcal{A}) = 1] - \tfrac{1}{2}\Bigr|$$
is negligible, where $\mathsf{Exp}^{\mathrm{prf}}_{\mathsf{VF}}$ is the standard VRF pseudorandomness game: $\mathcal{A}_1$ receives $\mathsf{pk}$ and an evaluation oracle, commits to a fresh challenge $x^*$, and $\mathcal{A}_2$ must distinguish $\mathsf{Eval}^y(\mathsf{sk}, x^*)$ from $y \gets_\$ \mathcal{Y}$.

**Verifiable random function.** A VRF is a VF that is perfectly correct, uniquely provable and pseudorandom. The primitive $\mathsf{VRF}$ has $|\mathsf{VRF}| = 3$ algorithms.

**Robust $(n; k)$ black-box combiner.** Let $\mathsf{P}$ be a cryptographic primitive specified by $|\mathsf{P}|$ algorithms. A robust $(n; k)$ *black-box* $\mathsf{P}$-combiner is a ppt. oracle Turing machine $\Pi_{\mathsf{P}} = (\mathsf{Alg}_{\mathsf{P}}^1, \ldots, \mathsf{Alg}_{\mathsf{P}}^{|\mathsf{P}|})$ such that: if there exists a ppt. adversary $\mathcal{A}^{\mathcal{O}_1, \ldots, \mathcal{O}_n, \mathsf{PSPACE}}$ that breaks the security of $\Pi_{\mathsf{P}}[\mathcal{O}_1, \ldots, \mathcal{O}_n]$, then there exist ppt. adversaries $\mathcal{B}_{i_1}, \ldots, \mathcal{B}_{i_{n-k+1}}$ and an injective $\ell : [n-k+1] \to [n]$ such that each $\mathcal{B}_{i_j}$ breaks the security of candidate $\mathcal{O}_{\ell(j)}$. The combiner is required to have black-box access to each candidate (oracle queries only) and to be secure against adversaries that additionally have access to a $\mathsf{PSPACE}$-complete oracle; this latter requirement forces the combiner to derive its security from the candidates rather than from its own internal hardness.

**Robust $(n; k)$ black-box DAG combiner (normal form).** A robust $(n; k)$ black-box combiner in which every algorithm $\mathsf{Alg}_{\mathsf{P}}$ is specified by a tuple
$$\bigl(\mathcal{G}, s, t, \ell, \{f^v\}_{v \in V(\mathcal{G}) \setminus \{s\}}\bigr),$$
where $\mathcal{G}$ is a directed acyclic graph with distinguished source $s$ (no incoming edges) and sink $t$ (no outgoing edges), $\ell : V(\mathcal{G}) \setminus \{s, t\} \to [n] \times [|\mathsf{P}|]$ labels each intermediate vertex with a candidate-algorithm pair, and $f^v$ is a ppt. Turing machine *without* oracle access that computes the query input $\mathbf{q}_v$ from the outputs $\{\mathbf{x}_w\}_{w \in \mathcal{N}^-_{\mathcal{G}}(v)}$ of the predecessors. Execution proceeds in topological order: $\mathbf{q}_v \gets f^v(\{\mathbf{x}_w\}_{w \in \mathcal{N}^-_\mathcal{G}(v)})$, $\mathbf{x}_v \gets \mathcal{O}_{\ell(v)}(\mathbf{q}_v)$, and the final output is $f^t(\{\mathbf{x}_w\}_{w \in \mathcal{N}^-_\mathcal{G}(t)})$. By Theorem 3.X of the thesis (DAG equivalence), every robust $(n; k)$ black-box combiner is *computationally equivalent* to a robust $(n; k)$ black-box DAG combiner, so reasoning over DAG combiners is without loss of generality.

**Verification trace.** For a robust black-box DAG VRF-combiner, the *verification trace* $\tau$ of an execution of $\Pi_{\mathsf{VRF}}[\mathcal{O}_1, \ldots, \mathcal{O}_n].\mathsf{Vfy}_{\mathsf{VRF}}$ on input $(\mathsf{pk}, x, y, \pi)$ is the sequence of $(\ell(v), \mathbf{q}_v, \mathbf{x}_v)$ triples recorded at every vertex of $\mathcal{G}_{\mathsf{Vfy}}$ during that execution.

**Exposed and obscure queries.** Fix a combiner evaluation that produces $(y, \pi)$ on input $\mathsf{sk}, x$ and let $\tau$ be the verification trace of the corresponding verification. A vertex $v$ in the evaluation DAG $\mathcal{G}_{\mathsf{Eval}}$ is *exposed* if $(\ell(v), \mathbf{q}_v)$ appears in $\tau$, and *obscure* otherwise. Exposed queries are those whose answers the combiner has "committed to" via the verification procedure; obscure queries are free in the sense that the verifier cannot tell what the candidate answered.

## Problem statement

**What is known (this thesis).**

1. **DAG normal form.** Every robust $(n; k)$ black-box VRF-combiner is computationally equivalent to a robust $(n; k)$ black-box DAG VRF-combiner. This lets all impossibility arguments be phrased in terms of DAG executions and verification traces.

2. **Impossibility at $k = 1$ for $n = 2$.** There exists no robust $(2; 1)$ black-box VRF-combiner. The proof constructs candidates $\mathcal{O}_1, \mathcal{O}_2$, where $\mathcal{O}_2$ is a VRF and $\mathcal{O}_1$ is an insecure (uniquely provable, perfectly correct) VF, and exhibits a ppt.-with-$\mathsf{PSPACE}$ adversary that breaks the pseudorandomness of $\Pi_{\mathsf{VRF}}[\mathcal{O}_1, \mathcal{O}_2]$. The core step is the *obscure-query argument*: for any fixed image-proof pair $(y, \pi)$ verified under $\mathsf{pk}$ with trace $\tau$, the values returned at obscure queries to $\mathcal{O}_2$ in the evaluation DAG cannot influence the combiner's output without violating either unique provability or perfect correctness of $\Pi_{\mathsf{VRF}}[\mathcal{O}_1, \mathcal{O}_2]$. This collapses the combiner's pseudorandomness to a filtering game on secret-key candidates, which the adversary wins with non-negligible advantage.

3. **Impossibility of $(n; \lfloor n/2 \rfloor)$.** Any robust $(n; \lfloor n/2 \rfloor)$ black-box VRF-combiner yields a robust $(2; 1)$ black-box VRF-combiner by grouping candidates into two halves and running the combiner once on each ordering. By Result 2, no such combiner can exist — so at most half of the candidates being secure is already insufficient.

4. **Positive result under strong candidate assumptions.** If *all* candidates $\mathcal{O}_1, \ldots, \mathcal{O}_n$ are assumed to be uniquely provable and perfectly correct VFs, then a simple XOR-style robust $(n; 1)$ black-box VRF-combiner exists, constructed by extending the classical black-box PRF-combiner. This establishes a sharp contrast: it is the *uniqueness/correctness* guarantees on the candidates, not their pseudorandomness, that make combining possible.

**What is open.** The gap between the known impossibility at $k = \lfloor n/2 \rfloor$ and the trivial positive regime at $k = n$ is the central open question. In particular:

1. **(Main conjecture — strong impossibility)** For every $n \ge 2$, there exists no robust $(n; n-1)$ black-box VRF-combiner. Equivalently: even when *all but one* of the candidates are guaranteed to be secure VRFs, no black-box construction can build a single secure VRF from them. Quantitatively, one would like an infinite family $\{\mathcal{A}_n\}_{n \ge 2}$ of ppt.-with-$\mathsf{PSPACE}$ adversaries such that for every candidate robust $(n; n-1)$ black-box VRF-combiner $\Pi_{\mathsf{VRF}}$, there exist candidates $\mathcal{O}_1, \ldots, \mathcal{O}_n$ (with exactly one insecure) under which $\mathcal{A}_n$ wins the pseudorandomness game against $\Pi_{\mathsf{VRF}}[\mathcal{O}_1, \ldots, \mathcal{O}_n]$ with non-negligible advantage.

2. **(Obstruction analysis)** The $(2; 1)$ proof relies on a combinatorial structure peculiar to two candidates: a single obscure query to $\mathcal{O}_2$ can be isolated, and its freedom is directly converted into a unique-provability/correctness violation of the combiner. Identify whether this structural argument admits a generalization to $n$ candidates in which the single insecure candidate is *not known a priori* to the reduction. Specifically, is there a generalization of the *obscure query* notion to DAG executions over $n$ candidates under which the adversary can simulate the combiner's evaluation on all $n-1$ candidate choices for the insecure oracle simultaneously, while exploiting the unique-provability/perfect-correctness commitment from the verification trace? A characterization of which DAG topologies admit such a simulation would directly settle Conjecture 1.

3. **(Intermediate regime — tight $k$ threshold)** Characterize the set $K^*(n) \subseteq [n]$ of values $k$ for which a robust $(n; k)$ black-box VRF-combiner exists. The known results yield $K^*(n) \subseteq (\lfloor n/2 \rfloor, n]$ and $n \in K^*(n)$ under strong candidate assumptions only. Is $K^*(n) = \{n\}$ (strong impossibility), is there a phase transition at some $k^*(n) > \lfloor n/2 \rfloor$, or does $k^*(n)$ depend non-trivially on auxiliary candidate assumptions (e.g., statistical vs. perfect correctness, uniqueness under $\mathsf{PSPACE}$ vs. computational uniqueness)?

4. **(Relaxations that circumvent the impossibility)** For each of the following relaxations of the robust black-box VRF-combiner definition, determine whether a robust $(n; 1)$ construction exists: (a) combined scheme satisfies only statistical correctness; (b) combined scheme satisfies only computational unique provability; (c) candidates are assumed to be verifiable unpredictable functions (VUFs) or weak VRFs instead of VRFs; (d) the combiner operates on richer VRF notions such as constrainable, key-homomorphic, aggregable, or unbiasable VRFs, where additional algebraic structure may admit non-black-box-style interpolation between candidates.

**Motivation.** Settling Conjecture 1 would complete the black-box combiner picture for VRFs, placing VRFs on the opposite extreme from PRFs: whereas a single secure PRF candidate suffices to build a secure combined PRF, a single *insecure* VRF candidate is enough to break any black-box VRF-combiner. This would sharpen the folklore intuition that verifiability is intrinsically hostile to black-box interpolation, and would motivate non-black-box or structure-aware combiner constructions for VRF-based protocols in post-quantum and hybrid cryptography — in particular for Proof-of-Stake leader election, lotteries and e-cash systems whose security currently rests on a single VRF assumption.

## References

- Rohner, "On Combiners for Verifiable Random Functions", MSc thesis, ETH Zürich, 2026. (Establishes the DAG normal form, the $(2; 1)$ impossibility with the obscure-query argument, the $(n; \lfloor n/2 \rfloor)$ corollary, and the positive XOR-combiner under strong candidate assumptions — the direct starting point for the conjecture.)
- Micali, Rabin, Vadhan, "Verifiable Random Functions", FOCS 1999. (Original definition of VRFs and VUFs.)
- Harnik, Kilian, Naor, Reingold, Rosen, "On Robust Combiners for Oblivious Transfer and Other Primitives", EUROCRYPT 2005. (Introduces robust $(n; k)$ black-box combiners and establishes the framework; gives the folklore PRF combiner and the PSPACE-relative security convention.)
- Brandt, "On the Security of Verifiable Random Functions" (working title, 2025). (Source of the unique provability and pseudorandomness formulations used in the thesis; relevant for characterizing candidate assumptions under which combiners are possible.)
- Hofheinz, Jager, "Verifiable Random Functions from Standard Assumptions", TCC 2016. (Standard-assumption VRF constructions; informs which candidate distributions are natural in the impossibility setting.)
- Ananth, Jain, Naor, Sahai, Yogev, "Universal Constructions and Robust Combiners for Indistinguishability Obfuscation and Witness Encryption", CRYPTO 2016. (Demonstrates black-box combiner techniques for primitives with verifiability-like constraints — a potential source of non-black-box analogues for VRFs.)
- Chakraborty, Magri, Venturi, Xagawa, "Robust Combiners for Non-Interactive Zero-Knowledge", ePrint 2025/XXXX. (Recent work on black-box combiners for verifiable primitives; methodologically closest to a prospective $(n; n-1)$ VRF impossibility proof.)
