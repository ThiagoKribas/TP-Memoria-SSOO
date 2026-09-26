<!-- FILE: lang-formal-foundations.md -->
# Formal Languages Foundations & String Operations

## 1. Formal Definitions & Mathematical Structures

### 1.1 Alphabet ($\Sigma$)
An **alphabet** $\Sigma$ is a finite, non-empty set of symbols.
- Example: $\Sigma = \{a, b, c\}$ or $\Sigma = \{0, 1\}$.
- Non-empty constraint: $\Sigma 
eq \emptyset$.
- Finiteness constraint: $|\Sigma| < \infty$.

### 1.2 String / Word ($lpha, eta, w$)
A **string** (or word) over an alphabet $\Sigma$ is a finite sequence of symbols chosen from $\Sigma$.
- Empty String ($\lambda$ or $\epsilon$): The sequence containing zero symbols ($|\lambda| = 0$).
  - Note: $\lambda 
otin \Sigma$, but $\lambda \in \Sigma^*$.
  - Note: $\{\lambda\} 
eq \emptyset$. The set $\{\lambda\}$ contains one element (length 0 string), whereas $\emptyset$ is the empty language.

### 1.3 Alphabet Powers & Closures
For any alphabet $\Sigma$:
- **$n$-th Power ($\Sigma^n$):** The set of all strings of exact length $n$ over $\Sigma$.
  - $|\Sigma^n| = |\Sigma|^n$.
  - Base case: $\Sigma^0 = \{\lambda\}$.
- **Kleene Closure ($\Sigma^*$):** The infinite set of all finite-length strings over $\Sigma$.
  $$\Sigma^* = igcup_{n \ge 0} \Sigma^n$$
  - $\Sigma^*$ is countably infinite ($\exists f: \mathbb{N} 	o \Sigma^*$ bijective).
- **Positive Closure ($\Sigma^+$):** The set of all non-empty strings over $\Sigma$.
  $$\Sigma^+ = igcup_{n \ge 1} \Sigma^n = \Sigma^* \setminus \{\lambda\}$$
  - Relation: $\Sigma^+ \subseteq \Sigma^*$, and $\Sigma^+ = \Sigma^*$ if and only if $\lambda \in \Sigma^+$ (which is false by definition).

---

## 2. String Operations & Algebraic Properties

### 2.1 String Concatenation ($lpha \cdot eta$)
Given $lpha, eta \in \Sigma^*$, the concatenation $lpha \cdot eta$ yields a string containing the symbols of $lpha$ followed sequentially by the symbols of $eta$.

#### Structural Definition of Strings & Recursive Length Function:
Every string $lpha \in \Sigma^*$ is structurally defined as either:
1. Base case: $\lambda$ (empty string).
2. Inductive case: $x \cdot lpha_1$, where $x \in \Sigma$ and $lpha_1 \in \Sigma^*$.

**Recursive Length Function $|\cdot|: \Sigma^* 	o \mathbb{N}_0$:**
$$|lpha| = egin{cases} 0 & 	ext{if } lpha = \lambda \ 1 + |lpha_1| & 	ext{if } lpha = x \cdot lpha_1 	ext{ with } x \in \Sigma, lpha_1 \in \Sigma^* \end{cases}$$

#### Properties of Concatenation:
- **Length Additivity:** $|lpha \cdot eta| = |lpha| + |eta|$ (proven by structural induction on $lpha$).
- **Power Additivity:** $|lpha^n| = n \cdot |lpha|$.
- **Identity Element:** $\lambda$ is the unique neutral element ($lpha \cdot \lambda = \lambda \cdot lpha = lpha$).
- **Associativity:** $(lpha \cdot eta) \cdot \gamma = lpha \cdot (eta \cdot \gamma)$.
- **Non-Commutativity:** In general, $lpha \cdot eta 
eq eta \cdot lpha$ (e.g., $ab 
eq ba$).

---

## 3. Language Definitions & Operations

### 3.1 Language Definition
A **language** $L$ over an alphabet $\Sigma$ is any subset of $\Sigma^*$ ($L \subseteq \Sigma^*$).
- Extreme examples: $L = \emptyset$ (empty language), $L = \{\lambda\}$ (language containing only empty string), $L = \Sigma^*$.

### 3.2 Fundamental Set & String Operations on Languages

| Operation | Formal Definition | Operational Specifications & Edge Cases |
| :--- | :--- | :--- |
| **Complement** ($L^c$ or $\overline{L}$) | $L^c = \Sigma^* \setminus L$ | Depends explicitly on the background alphabet $\Sigma$. E.g., for $L = \{a^n \mid n \ge 3\}$: over $\Sigma=\{a\}$, $L^c = \{\lambda, a, aa\}$; over $\Sigma=\{a,b\}$, $L^c = \{\lambda, a, aa\} \cup \{w \in \{a,b\}^* \mid \|w\|_b \ge 1\}$. |
| **Union** ($L_1 \cup L_2$) | $\{lpha \in \Sigma^* \mid lpha \in L_1 \lor lpha \in L_2\}$ | Both languages must share the same alphabet $\Sigma$. Neutral element: $\emptyset$. |
| **Intersection** ($L_1 \cap L_2$) | $\{lpha \in \Sigma^* \mid lpha \in L_1 \land lpha \in L_2\}$ | Neutral element: $\Sigma^*$. Absorbing element: $\emptyset$. |
| **Reverse** ($L^R$ or $L^r$) | $\{lpha^R \mid lpha \in L\}$ | Where $(x \cdot lpha_1)^R = lpha_1^R \cdot x$ and $\lambda^R = \lambda$. Double reverse identity: $(L^R)^R = L$. |
| **Concatenation** ($L_1 \cdot L_2$) | $\{lpha \cdot eta \mid lpha \in L_1 \land eta \in L_2\}$ | Neutral element: $\{\lambda\}$ ($L \cdot \{\lambda\} = L$). Absorbing element: $\emptyset$ ($L \cdot \emptyset = \emptyset$). Non-commutative. |
| **$n$-th Power** ($L^n$) | $L^0 = \{\lambda\}$, $L^n = L \cdot L^{n-1}$ for $n \ge 1$ | $L^0$ contains $\lambda$ regardless of whether $\lambda \in L$. |
| **Kleene Closure** ($L^*$) | $L^* = igcup_{n \ge 0} L^n$ | Always contains $\lambda$ ($\lambda \in L^*$). Idempotent property: $(L^*)^* = L^*$. |
| **Positive Closure** ($L^+$) | $L^+ = igcup_{n \ge 1} L^n$ | Note: $L^+ = L^*$ if and only if $\lambda \in L$. Otherwise $L^* = L^+ \cup \{\lambda\}$. |

---

## 4. Sub-string Relations & Structural Subsets

For a language $L \subseteq \Sigma^*$:

### 4.1 Prefixes ($Ini(L)$)
$$Ini(L) = \{lpha \in \Sigma^* \mid \exists eta \in \Sigma^* 	ext{ s.t. } lphaeta \in L\}$$
- Quitting zero or more symbols from the right.

### 4.2 Suffixes ($Fin(L)$)
$$Fin(L) = \{lpha \in \Sigma^* \mid \exists eta \in \Sigma^* 	ext{ s.t. } etalpha \in L\}$$
- Quitting zero or more symbols from the left.

### 4.3 Substrings ($Sub(L)$)
$$Sub(L) = \{lpha \in \Sigma^* \mid \exists eta, \gamma \in \Sigma^* 	ext{ s.t. } etalpha\gamma \in L\}$$

### 4.4 Inclusion Theorems & Edge Cases:
1. $L \subseteq Ini(L) \subseteq Sub(L) \subseteq \Sigma^*$.
2. $L \subseteq Fin(L) \subseteq Sub(L) \subseteq \Sigma^*$.
3. $\lambda \in Ini(L) \cap Fin(L)$ **if and only if** $L 
eq \emptyset$. (If $L = \emptyset$, then $Ini(\emptyset) = \emptyset$, so $\lambda 
otin Ini(\emptyset)$).
4. $Ini(Sub(L)) = Sub(L)$.
