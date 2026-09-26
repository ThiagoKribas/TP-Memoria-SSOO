<!-- FILE: automata-finite-dfa-nfa.md -->
# Deterministic & Nondeterministic Finite Automata (DFA & NFA)

## 1. Deterministic Finite Automata (DFA / AFD)

### 1.1 Formal Definition
A Deterministic Finite Automaton (DFA / AFD) is defined as a 5-tuple:
$$A = \langle Q, \Sigma, \delta, q_0, F angle$$
Where:
- $Q$: Finite, non-empty set of internal states.
- $\Sigma$: Finite alphabet of input symbols.
- $\delta: Q 	imes \Sigma 	o Q$: Total transition function mapping a single state and symbol to exactly one successor state.
- $q_0 \in Q$: Unique initial state.
- $F \subseteq Q$: Set of accepting (final) states.

### 1.2 Generalized Transition Function ($\hat{\delta}$)
Extends $\delta$ from single symbols to full strings $\Sigma^*$:
$$\hat{\delta} : Q 	imes \Sigma^* 	o Q$$
Defined recursively by:
1. **Base Case:** $\hat{\delta}(q, \lambda) = q$
2. **Inductive Step:** $\hat{\delta}(q, xa) = \delta(\hat{\delta}(q, x), a)$, for $x \in \Sigma^*$ and $a \in \Sigma$.

### 1.3 Instantaneous Configurations & Acceptance
- **Instantaneous Configuration (CI):** A pair $(q, w) \in Q 	imes \Sigma^*$, representing the current state $q$ and the unconsumed remaining input $w$.
- **Transition Step ($dash_A$):** $(q_i, a \cdot lpha) dash_A (q_j, lpha) \iff \delta(q_i, a) = q_j$.
- **Reflexive Transitive Closure ($dash_A^*$):** Zero or more configuration steps.
- **Accepted Language ($L(A)$):**
  $$L(A) = \{x \in \Sigma^* \mid \hat{\delta}(q_0, x) \in F\} = \{x \in \Sigma^* \mid (q_0, x) dash_A^* (q_f, \lambda) 	ext{ with } q_f \in F\}$$

### 1.4 Operational Constraint: Complete Transition Function & Trap State
- A DFA must be **complete**: $\delta(q, a)$ must be defined for every $q \in Q$ and $a \in \Sigma$.
- **Trap State ($q_T$ / Estado Trampa):** A non-accepting state ($q_T 
otin F$) with self-loop transitions $\delta(q_T, a) = q_T$ for all $a \in \Sigma$, used to complete partial transition functions.

---

## 2. Nondeterministic Finite Automata (NFA / AFND)

### 2.1 Formal Definition
A Nondeterministic Finite Automaton (NFA / AFND) is a 5-tuple:
$$M = \langle Q, \Sigma, \delta, q_0, F angle$$
Where:
- $Q, \Sigma, q_0, F$ are defined as in a DFA.
- $\delta: Q 	imes \Sigma 	o \mathcal{P}(Q)$: Transition function mapping a state and symbol to a **set of possible next states** (where $\mathcal{P}(Q)$ is the power set of $Q$).
  - If $\delta(q, a) = \emptyset$, the computation path terminates (dead end). In NFAs, implicit trap states are allowed ($\emptyset$ representation).

### 2.2 Generalized NFA Transition Function ($\hat{\delta}$)
$$\hat{\delta} : Q 	imes \Sigma^* 	o \mathcal{P}(Q)$$
Defined recursively by:
1. **Base Case:** $\hat{\delta}(q, \lambda) = \{q\}$
2. **Inductive Step:** $\hat{\delta}(q, xa) = \{p \in Q \mid \exists r \in \hat{\delta}(q, x) 	ext{ s.t. } p \in \delta(r, a)\}$

### 2.3 Set Extension of Transition Function
For a subset of states $P \subseteq Q$:
$$\delta_{ext}(P, a) = igcup_{q \in P} \delta(q, a), \quad \hat{\delta}_{ext}(P, x) = igcup_{q \in P} \hat{\delta}(q, x)$$

### 2.4 NFA Accepted Language
$$L(M) = \{x \in \Sigma^* \mid \hat{\delta}(q_0, x) \cap F 
eq \emptyset\}$$
- **Operational Logic:** A string $x$ is accepted if **at least one** computation path starting at $(q_0, x)$ consumes the entire string and ends in a state $q_f \in F$. Failing branches or dead ends do not reject the string if an accepting branch exists.

---

## 3. Equivalence of DFA and NFA: Subset Construction

### 3.1 Theorem (Rabin & Scott, 1959)
For every NFA $M = \langle Q, \Sigma, \delta, q_0, F angle$, there exists an equivalent DFA $M' = \langle Q', \Sigma, \delta', q_0', F' angle$ such that $L(M) = L(M')$.

### 3.2 Subset Construction Algorithm (Determinization)
Given NFA $M = \langle Q, \Sigma, \delta, q_0, F angle$:
1. **States of $M'$:** $Q' = \mathcal{P}(Q)$ (or the subset of reachable state-sets). Worst-case state count: $|Q'| = 2^{|Q|}$.
2. **Alphabet:** $\Sigma' = \Sigma$.
3. **Start State:** $q_0' = \{q_0\}$.
4. **Transition Function $\delta'$:** For each state-set $P \in Q'$ and $a \in \Sigma$:
   $$\delta'(P, a) = igcup_{q \in P} \delta(q, a)$$
5. **Accepting States $F'$:** $F' = \{P \in Q' \mid P \cap F 
eq \emptyset\}$.

### 3.3 Proof Strategy Sketch (Induction on String Length)
- **Base Case ($x = \lambda$):** $\hat{\delta}'(q_0', \lambda) = \{q_0\} = \hat{\delta}(q_0, \lambda)$.
- **Inductive Hypothesis:** Assume $\hat{\delta}'(q_0', x) = \hat{\delta}(q_0, x)$ holds for string $x$.
- **Inductive Step ($xa$):**
  $$\hat{\delta}'(q_0', xa) = \delta'(\hat{\delta}'(q_0', x), a) = \delta'(\hat{\delta}(q_0, x), a) = igcup_{q \in \hat{\delta}(q_0, x)} \delta(q, a) = \hat{\delta}(q_0, xa)$$
- **Acceptance Equivalence:**
  $$x \in L(M') \iff \hat{\delta}'(q_0', x) \in F' \iff \hat{\delta}(q_0, x) \cap F 
eq \emptyset \iff x \in L(M)$$
