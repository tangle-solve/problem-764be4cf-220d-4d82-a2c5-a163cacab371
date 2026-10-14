<!-- solve-problem:764be4cf-220d-4d82-a2c5-a163cacab371 -->
# Existence of one-way functions

A [Tangle Solve problem](https://solve.tangle.tools/problems/764be4cf-220d-4d82-a2c5-a163cacab371).

## What is the problem?

Prove or refute: there exists a function f : {0,1}* → {0,1}* computable in deterministic polynomial time such that no probabilistic polynomial-time (PPT) algorithm A inverts f with noticeable probability. Formally, f is one-way if:

1. There is a polynomial-time algorithm computing f(x) on input x.
2. For every PPT adversary A and every polynomial p, for all sufficiently large n,
   Pr_{x ← {0,1}^n}[f(A(1^n, f(x))) = f(x)] < 1/p(n).

The probability is over uniform x and A's internal randomness. "Noticeable" means ≥ 1/p(n) for some polynomial p; "negligible" means < 1/p(n) for every p. A standard equivalent variant (sometimes stronger-looking but equivalent under mild conditions) is the distributional version where the adversary must output x itself on uniformly random inputs, or must succeed on a 1/poly fraction of inputs.

Ambiguities to note: the problem is a sheer existence statement — one may exhibit any explicit f, or prove nonconstructively (e.g., via counting) that one-way functions exist relative to some complexity separation. A counting argument shows that *some* function is hard to invert if P ≠ NP-type separations hold, but the obstacle is that the hard function must simultaneously be *polynomial-time computable*. Any resolution must confront this: it would imply P ≠ NP (indeed much more: average-case hardness in NP).

## Why does it matter?

One-way functions are provably equivalent (via the Impagliazzo–Levin–Luby / Håstad–Impagliazzo–Levin–Luby machinery) to pseudorandom generators, pseudorandom functions, bit commitment, symmetric encryption, digital signatures, and private-key cryptography generally. Resolving existence positively would place all of modern cryptography on unconditional footing; resolving it negatively (i.e., proving all polynomial-time functions are invertible on average by PPT) would collapse the worlds of Impagliazzo's Five Worlds to "Algorithmica" and break essentially all computational cryptography.

## What's already known?

- **Equivalences:** OWFs exist iff pseudorandom generators exist (Håstad, Impagliazzo, Levin, Luby, 1999); iff pseudorandom functions, commitment schemes, and general symmetric crypto exist. So the problem is a single gateway to a large equivalence class.
- **Unconditional hardness is known for weaker objects:** one-way functions against AC⁰ circuits exist unconditionally, and average-case hardness against exponential-time algorithms for functions in E follows from the hardness-vs-randomness program (Impagliazzo–Wigderson). Time hierarchies give languages in, say, E not in P, but these are worst-case and the functions aren't directly invertible-hard in the OWF sense.
- **Relativization barriers:** Baker–Gill–Solovay-style oracles exist relative to which OWFs exist and relative to which they don't (relative to a PSPACE-complete oracle every PTIME function is invertible), so any proof must be non-relativizing; natural proofs (Razborov–Rudich) suggest standard circuit-lower-bound techniques may "prove too much" and fail.
- **Candidates:** multiplication/factoring, discrete log, subset-sum, lattice problems (SIS/LWE). All are assumptions, not theorems.
- **Partial progress:** OWFs from average-case hardness of specific NP problems (e.g., one-way permutations from worst-case lattice assumptions via Ajtai's work); Impagliazzo's "Five Worlds" framework (Austrin–Impagliazzo 2018 give oracle separations among the worlds).

## What would count as solving it?

**Positive resolution:** Exhibit (or nonconstructively prove the existence of) a polynomial-time computable f and a proof that every PPT A fails to invert it with non-negligible probability. A referee accepts: a fully rigorous proof, unconditional (no unproved assumptions), handling arbitrary adversaries including nonuniform advice if claimed.

**Negative resolution:** Prove that for every polynomial-time f there is a PPT A inverting f on a 1/poly fraction of outputs — implying, e.g., that factoring, discrete log, etc. are easy on average. This would be at least as revolutionary.

**Partial results of value:** unconditional OWFs against restricted adversary classes beyond AC⁰; barriers formalizing which proof techniques cannot work; equivalences to cleaner statements (e.g., hardness of a fixed natural distributional NP problem).

## How to start

1. Read Goldreich, *Foundations of Cryptography Vol. 1*, Chapter 2 for the precise definitional choices (strong vs. weak OWFs, the amplification theorem showing weak implies strong via the "hard-core set"/direct product argument).
2. Read Håstad–Impagliazzo–Levin–Luby (1999) to understand exactly what OWFs buy and where they're used, which clarifies what a proof must avoid.
3. Internalize the barriers: relativization (via the PSPACE oracle where all PTIME functions are invertible) and Razborov–Rudich natural proofs — any proposed technique that yields circuit lower bounds "naturalistically" is suspect.
4. Natural attack: try to derive OWFs from an average-case version of the time hierarchy theorem, or from explicit hitting sets / error-correcting codes; the persistent obstacle is that worst-case hardness is easy to get but average-case (inversion) hardness against *uniform* probabilistic algorithms has no known boosting route downward from hierarchy theorems.

## Key references

- W. Diffie, M. Hellman (1976), "New Directions in Cryptography," IEEE Trans. Inf. Theory 22(6). Introduced the computational-complexity foundation for public-key cryptography and the notion that inversion could be provably hard.
- O. Goldreich (2001), *Foundations of Cryptography: Basic Tools*, Vol. 1, Cambridge UP. Definitive treatment of one-way function definitions, weak/strong equivalence, and their consequences.
- J. Håstad, R. Impagliazzo, L. Levin, M. Luby (1999), "A Pseudorandom Generator from any One-way Function," SIAM J. Comput. 28(4). Proves the equivalence of OWFs and pseudorandom generators, the central structural result around this problem.
- R. Impagliazzo (1995), "A Personal View of Average-Case Complexity,"Structure in Complexity Theory. The "Five Worlds" framework situating the problem's stakes.
- A. Razborov, S. Rudich (1997), "Natural Proofs," JCSS 55(1). Barrier result: standard circuit-lower-bound techniques cannot prove hardness useful for pseudorandomness. (verify relevance level: indirect barrier)
- M. Ajtai (1996), "Generating Hard Instances of Lattice Problems," STOC. Worst-case-to-average-case reduction for lattices — the best-known unconditional analogue of what's needed. (verify)

## Warnings

- Any proof relativizes incorrectly if it would also work relative to a PSPACE-complete oracle (relative to which all PTIME functions are PTIME-invertible); check oracle closure early.
- Do not conflate "P ≠ NP" (worst-case) with the needed average-case hardness; P ≠ NP does not imply OWFs by any known argument, and Impagliazzo's worlds (Heuristica vs. Minicrypt) separate them oracle-relatively.
- Beware definitional traps: weak vs. strong OWFs, nonuniform (circuits) vs. uniform adversaries, and whether the adversary gets 1^n; all standard formulations are in Goldreich Ch. 2.
- "Natural proofs" means a naive circuit-lower-bound proof of OWF existence would break cryptography itself — a built-in contradiction signal.
- Counting arguments show most functions are one-way, but not polynomial-time computable ones; this dead end recurs constantly.

---
*`ALGO-000198` · unverified*

## Acceptance guidance

**Positive resolution:** Exhibit (or nonconstructively prove the existence of) a polynomial-time computable f and a proof that every PPT A fails to invert it with non-negligible probability. A referee accepts: a fully rigorous proof, unconditional (no unproved assumptions), handling arbitrary adversaries including nonuniform advice if claimed.

**Negative resolution:** Prove that for every polynomial-time f there is a PPT A inverting f on a 1/poly fraction of outputs — implying, e.g., that factoring, discrete log, etc. are easy on average. This would be at least as revolutionary.

**Partial results of value:** unconditional OWFs against restricted adversary classes beyond AC⁰; barriers formalizing which proof techniques cannot work; equivalences to cleaner statements (e.g., hardness of a fixed natural distributional NP problem).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/764be4cf-220d-4d82-a2c5-a163cacab371?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000198.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000198)
