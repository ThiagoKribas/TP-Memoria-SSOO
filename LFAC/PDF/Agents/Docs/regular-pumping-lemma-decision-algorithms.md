<!-- FILE: regular-pumping-lemma-decision-algorithms.md -->
# Pumping Lemma & Decision Algorithms for Regular Languages

## 1. Pumping Lemma for Regular Languages

### 1.1 Formal Statement (Scott & Rabin 1959, Bar-Hillel et al. 1961)
Let $L$ be a regular language. Then there exists a pumping length constant $n \in \mathbb{N}^+$ (typically $n = |Q|$ of a DFA recognizing $L$) such that for every string $z \in L$ with $|z| \ge n$, there exists a decomposition $z = u \cdot v \cdot w$ satisfying:
1. $|uv| \le n$
2. $|v| \ge 1$ ($v 
eq \lambda$)
3. $orall i \ge 0, \quad u \cdot v^i \cdot w \in L$

### 1.2 Proof via Pigeonhole Principle
Let $M = \langle Q, \Sigma, \delta, q_0, F angle$ be a DFA with $n = |Q|$ states that accepts $L$.
Let $z = a_1 a_2 \dots a_m \in L$ with $m \ge n$.
- Processing $z$ visits a sequence of $m + 1$ states: $q_{\ell_0}, q_{\ell_1}, \dots, q_{\ell_m}$, where $q_{\ell_0} = q_0$ and $q_{\ell_m} \in F$.
- Since $m + 1 > n$, by the **Pigeonhole Principle**, among the first $n + 1$ states ($q_{\ell_0}, \dots, q_{\ell_n}$), at least two states must be identical.
- Let $j, k$ be indices such that $0 \le j < k \le n$ and $q_{\ell_j} = q_{\ell_k}$.
- Partition $z = u \cdot v \cdot w$:
  - $u = a_1 \dots a_j$ (transitions $q_0 	o^* q_{\ell_j}$)
  - $v = a_{j+1} \dots a_k$ (loop transitions $q_{\ell_j} 	o^+ q_{\ell_k} = q_{\ell_j}$)
  - $w = a_{k+1} \dots a_m$ (transitions $q_{\ell_k} 	o^* q_{\ell_m} \in F$)
- Properties hold:
  1. $k \le n \implies |uv| \le n$.
  2. $j < k \implies |v| \ge 1$.
  3. Traversing the loop $v$ $i$ times yields $\hat{\delta}(q_0, u v^i w) = q_{\ell_m} \in F \implies u v^i w \in L$ for all $i \ge 0$.

```
      u = a1...aj         v = aj+1...ak (LOOP)        w = ak+1...am
  (q0) -------------> ( q_j = q_k ) ----------------> ( q_m ∈ F )
                            |_______v_______|
```

### 1.3 Strategic Game Formalism (Adversary Choice)
To prove $L$ is **NOT regular** using the Pumping Lemma, use proof by contradiction:
1. **Assume** $L$ is regular. Let $n$ be the pumping constant given by the lemma.
2. **Choose** a specific string $z \in L$ parameterized by $n$ such that $|z| \ge n$.
3. **Adversary Partition:** The adversary splits $z = uvw$ satisfying $|uv| \le n$ and $|v| \ge 1$. You must handle **ALL valid decompositions** satisfying these constraints without assuming a specific split.
4. **Pump Selection:** Pick an index $i \ge 0$ (frequently $i = 0$ or $i = 2$) such that $u v^i w 
otin L$.
5. **Conclude:** Contradiction! Thus $L$ is not regular.

---

## 2. Classic Non-Regularity Proofs

| Language ($L$) | Choice of $z \in L$ ($|z| \ge n$) | Adversary Constraints & Case Analysis | Pump Choice ($i$) & Contradiction |
| :--- | :--- | :--- | :--- |
| **$L_1 = \{0^k 1^k \mid k \ge 0\}$** | $z = 0^n 1^n$ | Since $|uv| \le n$, $v$ consists **exclusively of $0$s** ($v = 0^m, 1 \le m \le n$). | $i = 2 \implies u v^2 w = 0^{n+m} 1^n 
otin L_1$ (more 0s than 1s). |
| **$L_2 = \{0^{k^2} \mid k \ge 1\}$** | $z = 0^{n^2}$ | $v = 0^m$ with $1 \le m \le n$. | $i = 2 \implies |u v^2 w| = n^2 + m$. Since $1 \le m \le n$, $n^2 < n^2 + m \le n^2 + n < (n+1)^2$. Not a perfect square. |
| **$L_3 = \{a^p \mid p 	ext{ is prime}\}$** | Choose prime $p \ge n$, $z = a^p$. | $v = a^m$ with $1 \le m \le n$. Thus $|u w| = p - m$. | Choose $i = p + 1$. Length of $u v^{p+1} w$ is $|u w| + (p+1)m = (p - m) + pm + m = p(1 + m)$. Since $m \ge 1 \implies 1 + m \ge 2$, $p(1+m)$ is composite. |
| **$L_4 = \{w w^R \mid w \in \{0,1\}^*\}$** | $z = 0^n 1 1 0^n$ | $|uv| \le n \implies v$ is within the initial $0^n$ block ($v = 0^m$). | $i = 0 \implies u w = 0^{n-m} 1 1 0^n 
otin L_4$ (asymmetrical 0 counts). |
| **$L_5 = \{0^a 1^b \mid a 
eq b\}$** | Prove via Complement / Closure: $\overline{L_5} \cap 0^* 1^* = \{0^k 1^k \}$. | Alternatively: pump $L_5^c$ using string $z = 0^n 1^n \in L_5^c$. | $i = 2 \implies 0^{n+m} 1^n \in L_5$, contradicting $L_5^c$ closure under pumping. |

---

## 3. Decision Algorithms for Regular Languages

A problem is **decidable** if there exists an algorithm that always halts with a correct boolean (YES/NO) answer.

### 3.1 Membership Problem ($w \in L$)
- **Input:** DFA $M = \langle Q, \Sigma, \delta, q_0, F angle$ and string $w \in \Sigma^*$.
- **Algorithm:** Simulate $M$ on $w$ step-by-step to compute $\hat{\delta}(q_0, w)$.
- **Decision Rule:** Output YES if $\hat{\delta}(q_0, w) \in F$, else NO.
- **Time Complexity:** $O(|w|)$.

### 3.2 Emptiness Problem ($L(M) = \emptyset$)
- **Input:** DFA $M = \langle Q, \Sigma, \delta, q_0, F angle$ with $|Q| = n$.
- **Theorem:** $L(M) 
eq \emptyset \iff \exists w \in \Sigma^*$ s.t. $\hat{\delta}(q_0, w) \in F$ and $|w| < n$.
- **Algorithm 1 (Graph Reachability):** Compute reachable states $A$ from $q_0$ using BFS/DFS. Return YES if $A \cap F = \emptyset$.
- **Algorithm 2 (String Length Bound):** Test all strings $w \in \Sigma^*$ with $|w| < n$.

### 3.3 Finiteness Problem ($|L(M)| < \infty$)
- **Input:** DFA $M = \langle Q, \Sigma, \delta, q_0, F angle$ with $|Q| = n$.
- **Theorem:** $L(M)$ is infinite $\iff \exists z \in L(M)$ such that $n \le |z| < 2n$.
- **Algorithm 1 (Cycle Detection):** Prune states unreachable from $q_0$ or unable to reach $F$. Check if the remaining transition graph contains a directed cycle. Return YES if no cycle exists.
- **Algorithm 2 (Pumping Bound Check):** Test strings of length $n \le |w| < 2n$.

### 3.4 Equivalence Problem ($L(M_1) = L(M_2)$)
- **Input:** DFAs $M_1, M_2$.
- **Algorithm:** Construct the symmetric difference automaton $M_{diff}$ accepting $L_{diff} = (L_1 \cap \overline{L_2}) \cup (\overline{L_1} \cap L_2)$.
- **Decision Rule:** Run the Emptiness algorithm on $M_{diff}$. $L(M_1) = L(M_2) \iff L(M_{diff}) = \emptyset$.
