<!-- FILE: automata-pushdown-pda-cfl.md -->
# Pushdown Automata (PDA / AP) & Context-Free Languages

## 1. Pushdown Automaton Formal Definition

A **Pushdown Automaton (PDA / AP)** extends a finite automaton with an unbounded Last-In, First-Out (LIFO) stack storage mechanism.

### 1.1 Formal 7-Tuple Definition
$$M = \langle Q, \Sigma, \Gamma, \delta, q_0, Z_0, F angle$$
Where:
- $Q$: Finite set of control states.
- $\Sigma$: Input alphabet.
- $\Gamma$: Stack alphabet.
- $\delta: Q 	imes (\Sigma \cup \{\lambda\}) 	imes \Gamma 	o \mathcal{P}_{finite}(Q 	imes \Gamma^*)$: Transition function.
- $q_0 \in Q$: Initial state.
- $Z_0 \in \Gamma$: Initial stack symbol.
- $F \subseteq Q$: Set of final/accepting states.

### 1.2 Transition Function Semantics
An entry $(p, \gamma) \in \delta(q, a, Z)$ means:
1. Current state is $q$, current input symbol inspected is $a \in \Sigma \cup \{\lambda\}$, top stack symbol is $Z \in \Gamma$.
2. The top symbol $Z$ is **popped** from the stack.
3. The string $\gamma = c_1 c_2 \dots c_k \in \Gamma^*$ is **pushed** onto the stack such that $c_1$ becomes the new top-of-stack.
4. Input head advances past $a$ (if $a \in \Sigma$; stays fixed if $a = \lambda$).
5. State transitions to $p$.

---

## 2. Configurations & Acceptance Modes

### 2.1 Instantaneous Configuration (CI)
A triple representing a snapshot of execution:
$$(q, w, \gamma) \in Q 	imes \Sigma^* 	imes \Gamma^*$$
- $q$: Current state.
- $w$: Remaining unconsumed input string.
- $\gamma$: Full stack contents, written with top-of-stack at the leftmost position (e.g., $\gamma = Z \pi$).

### 2.2 Transition Step Relation ($dash$)
For $a \in \Sigma \cup \{\lambda\}, w \in \Sigma^*, Z \in \Gamma, \pi, \gamma \in \Gamma^*$:
$$(q, aw, Z\pi) dash (p, w, \gamma\pi) \iff (p, \gamma) \in \delta(q, a, Z)$$

### 2.3 Dual Modes of Acceptance

#### Mode 1: Acceptance by Final State ($L(M)$)
$$L(M) = \{w \in \Sigma^* \mid \exists p \in F, \gamma \in \Gamma^* 	ext{ s.t. } (q_0, w, Z_0) dash^* (p, \lambda, \gamma)\}$$
- Contents of the stack upon string consumption are disregarded.

#### Mode 2: Acceptance by Empty Stack ($L_\lambda(M)$)
$$L_\lambda(M) = \{w \in \Sigma^* \mid \exists p \in Q 	ext{ s.t. } (q_0, w, Z_0) dash^* (p, \lambda, \lambda)\}$$
- Final state set $F$ is irrelevant.

#### Equivalence Theorem:
$$L(M) 	ext{ accepted by Final State} \iff L_\lambda(M') 	ext{ accepted by Empty Stack}$$

---

## 3. Determinism vs. Non-determinism in PDAs

### 3.1 Deterministic Pushdown Automaton (DPDA) Definition
A PDA $M = \langle Q, \Sigma, \Gamma, \delta, q_0, Z_0, F angle$ is **deterministic** if and only if for all $q \in Q, A \in \Gamma$:
1. $|\delta(q, a, A)| \le 1$ for all $a \in \Sigma$.
2. $|\delta(q, \lambda, A)| \le 1$.
3. If $|\delta(q, \lambda, A)| = 1$, then $|\delta(q, a, A)| = 0$ for all $a \in \Sigma$.

### 3.2 Expressive Power Hierarchy & Separation
Unlike finite automata (where NFA = DFA in expressive power), **Nondeterministic PDAs are strictly more powerful than Deterministic PDAs**.

$$	ext{Regular Languages} \subsetneq 	ext{DCFL (Accepted by DPDA)} \subsetneq 	ext{CFL (Accepted by NPDA)}$$

- **Example Separation Language:** $L = \{w w^R \mid w \in \{0,1\}^*\}$ (even-length palindromes).
  - $L$ is a Context-Free Language recognized by an NPDA (which non-deterministically guesses the midpoint of string $w w^R$).
  - $L$ **cannot** be recognized by any DPDA because a DPDA cannot determine when the midpoint occurs without looking ahead non-deterministically.

---

## 4. Non-Context-Free Languages (Expressive Boundaries)

Standard examples of languages exceeding PDA capabilities (Type 1 Context-Sensitive):
1. $L_{abc} = \{a^n b^n c^n \mid n \ge 1\}$: Stack can match $a^n$ against $b^n$, but stack is empty when encountering $c^n$, losing count $n$.
2. $L_{ww} = \{w w \mid w \in \{a,b\}^*\}$: Stack pops symbols in reverse LIFO order ($w^R$), rendering comparison against non-reversed $w$ impossible with a single stack.
