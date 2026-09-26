<!-- FILE: 00_INDEX.md -->
# Knowledge Base Routing Index

| Filename | Scope / Key Contents | Primary Dependencies / Related Files |
| :--- | :--- | :--- |
| `lang-formal-foundations.md` | Formal definitions of alphabets, words, empty string $\lambda$, Kleene/positive closure, concatenation, language complement, union, intersection, reverse, powers, prefixes, suffixes, substrings. | Self-contained / Baseline foundations |
| `automata-finite-dfa-nfa.md` | Formal specs of DFA and NFA, generalized transitions $\hat{\delta}$, configurations $dash$, trap states, language acceptance, Subset Construction / Power Set determinization algorithm. | `lang-formal-foundations.md` |
| `automata-nfa-lambda-closure-properties.md` | $	ext{NFA-}\lambda$ definitions, $\lambda$-closure $Cl_\lambda(q)$, $\lambda$-elimination algorithm to NFA/DFA, Regular Language closure algebra (union, product intersection, complete DFA complement, reversal). | `automata-finite-dfa-nfa.md` |
| `automata-dfa-minimization-algorithms.md` | Accessible states search, $k$-indistinguishability $k\equiv$, Moore's $O(n^2 s)$ partition refinement algorithm, Hopcroft $O(n s \log n)$, Brzozowski double-reversal $O(2^n)$, minimal DFA uniqueness. | `automata-finite-dfa-nfa.md` |
| `regular-pumping-lemma-decision-algorithms.md` | Pumping Lemma for Regular Languages statement & Pigeonhole proof, adversary game strategy, non-regularity proofs ($0^n 1^n$, $a^{k^2}$, primes, palindromes), decidability algorithms (membership, emptiness, finiteness, equivalence). | `automata-finite-dfa-nfa.md`, `automata-dfa-minimization-algorithms.md` |
| `automata-pushdown-pda-cfl.md` | Pushdown Automata (PDA/AP) 7-tuple specs, LIFO stack operations, configurations $(q, w, \gamma)$, final state vs empty stack acceptance equivalence, DPDA vs NPDA expressive power separation ($w w^R$), Non-CFL boundary limits. | `lang-formal-foundations.md`, `automata-finite-dfa-nfa.md` |
| `grammars-cfg-chomsky-hierarchy.md` | Formal Grammars 4-tuple, Chomsky Hierarchy (Types 0, 1, 2, 3), CFG definitions, derivations $\Rightarrow$, parse trees, Chomsky/Greibach Normal Forms, CFG-PDA equivalence theorem. | `lang-formal-foundations.md`, `automata-pushdown-pda-cfl.md` |
| `cfl-pumping-lemma-closure-decision.md` | Pumping Lemma for Context-Free Languages statement & parse tree height proof, non-CFL proofs ($a^n b^n c^n$, $w w$, $a^{m^2}$), CFL closure properties ($CFL \cap Regular \in CFL$, non-closure under intersection/complement), CYK algorithm, undecidable CFG problems. | `automata-pushdown-pda-cfl.md`, `grammars-cfg-chomsky-hierarchy.md` |
