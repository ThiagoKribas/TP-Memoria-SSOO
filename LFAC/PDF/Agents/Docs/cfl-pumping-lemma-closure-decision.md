<!-- FILE: cfl-pumping-lemma-closure-decision.md -->
# Pumping Lemma, Closure Properties & Decision Algorithms for CFLs

## 1. Pumping Lemma for Context-Free Languages

### 1.1 Formal Statement
Let $L$ be a Context-Free Language. Then there exists a constant $n \in \mathbb{N}^+$ such that for every string $lpha \in L$ with $|lpha| \ge n$, there exists a decomposition $lpha = r \cdot x \cdot y \cdot z \cdot s$ satisfying:
1. $|xyz| \le n$
2. $|xz| \ge 1$ ($x$ and $z$ are not both empty)
3. $orall i \ge 0, \quad r \cdot x^i \cdot y \cdot z^i \cdot s \in L$

```
                   Parse Tree Structure for CFL Pumping Lemma:
                                     (S)
                                    / |                                    r (A) s
                                    / |                                    x (A) z
                                     |
                                     y
```

### 1.2 Non-CFL Proof Demonstrations

| Target Non-CFL ($L$) | Choice of String $lpha \in L$ | Substring Constraint $|xyz| \le n$ Analysis | Contradiction via Pump ($i = 0$ or $i = 2$) |
| :--- | :--- | :--- | :--- |
| **$L_1 = \{a^k b^k c^k \mid k \ge 0\}$** | $lpha = a^n b^n c^n$ | $xyz$ cannot contain $a$s, $b$s, and $c$s simultaneously (since $|xyz| \le n$). $xz$ contains at most two symbol types. | $i = 0 \implies r y s = r x^0 y z^0 s$. Deleting $x, z$ reduces counts of 1 or 2 symbol types while leaving the 3rd symbol count unchanged. String no longer has equal counts of $a, b, c$. |
| **$L_2 = \{w w \mid w \in \{a,b\}^*\}$** | $lpha = a^n b^n a^n b^n$ | $xyz$ spans across at most two adjacent blocks within $a^n b^n a^n b^n$. | $i = 2 \implies r x^2 y z^2 s$ alters symbol distribution in first half relative to second half, breaking $w w$ symmetry. |
| **$L_3 = \{a^{m^2} \mid m \ge 0\}$** | $lpha = a^{n^2}$ | $|xz| \le n$. Pumped string length for $i = 2$: $n^2 < |r x^2 y z^2 s| \le n^2 + n < (n+1)^2$. | Not a perfect square. |

---

## 2. Closure Properties of Context-Free Languages

### 2.1 Closed Operations
Class of CFLs is **CLOSED** under:
1. **Union ($L_1 \cup L_2$):** $G = \langle V_{N1} \cup V_{N2} \cup \{S\}, V_T, P_1 \cup P_2 \cup \{S 	o S_1 \mid S_2\}, S angle$.
2. **Concatenation ($L_1 \cdot L_2$):** $G = \langle V_{N1} \cup V_{N2} \cup \{S\}, V_T, P_1 \cup P_2 \cup \{S 	o S_1 S_2\}, S angle$.
3. **Kleene Star ($L^*$):** $G' = \langle V_N \cup \{S'\}, V_T, P \cup \{S' 	o S S' \mid \lambda\}, S' angle$.
4. **Reversal ($L^R$):** Reverse body of every rule: $X 	o lpha \implies X 	o lpha^R$.
5. **Intersection with Regular Language ($CFL \cap Regular \in CFL$):** Construct product machine combining PDA $M$ and DFA $A$. Stack operations mirror $M$ while control state tracks pair $(q_{PDA}, q_{DFA})$.

### 2.2 Non-Closed Operations
Class of CFLs is **NOT CLOSED** under:
1. **Intersection ($L_1 \cap L_2$):**
   - Counterexample: $L_1 = \{a^i b^j c^j\} \in CFL$, $L_2 = \{a^i b^i c^j\} \in CFL$.
   - $L_1 \cap L_2 = \{a^n b^n c^n\} 
otin CFL$.
2. **Complement ($\overline{L}$):**
   - If CFL were closed under complement, by De Morgan's Laws $L_1 \cap L_2 = \overline{\overline{L_1} \cup \overline{L_2}}$ would be in CFL, contradicting non-closure of intersection.
3. **Difference ($L_1 \setminus L_2$):**
   - $\Sigma^* \setminus L = \overline{L}$. If difference were closed, complement would be closed.

---

## 3. Decision Algorithms & Undecidability Bounded Matrix

### 3.1 Decidable Algorithms for CFGs
- **Membership ($w \in L(G)$):** Decidable via **CYK (Cocke-Younger-Kasami)** algorithm in $O(|w|^3 \cdot |G|)$ time using Chomsky Normal Form.
- **Emptiness ($L(G) = \emptyset$):** Decidable via non-terminal reachability/generation algorithm.
  ```python
  # Reachability algorithm for CFG Emptiness
  N_0 = empty
  repeat:
      N_i = N_{i-1} union { A | A -> alpha in P and alpha in (N_{i-1} union V_T)* }
  until N_i == N_{i-1}
  return (S in N_i) # YES if generated, NO if empty
  ```
- **Finiteness ($|L(G)| < \infty$):** Decidable via dependency graph cycle analysis on productive variables.

### 3.2 Undecidable Problems for CFGs
The following questions **CANNOT** be solved by any general algorithm (Undecidable / Incomputable):
1. Is a given CFG $G$ **ambiguous**?
2. Is $L(G_1) = L(G_2)$ for two arbitrary CFGs?
3. Is $L(G) = \Sigma^*$ (Universal Language)?
4. Is $L(G_1) \subseteq L(G_2)$?
5. Is $L(G_1) \cap L(G_2) = \emptyset$?
