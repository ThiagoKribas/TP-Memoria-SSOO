<!-- FILE: automata-dfa-minimization-algorithms.md -->
# DFA Minimization Theory & Algorithms

## 1. Reachability & Indistinguizability Theory

### 1.1 Accessible States
A state $p \in Q$ of a DFA $M = \langle Q, \Sigma, \delta, q_0, F angle$ is **accessible** if there exists $w \in \Sigma^*$ such that $\hat{\delta}(q_0, w) = p$.
- Unreachable states do not affect $L(M)$ and must be pruned prior to state minimization.

#### Reachability Computation Algorithm:
```python
# Pseudocode for computing accessible states
accessible = {q0}
new_states = {q0}
while new_states != empty:
    temp = empty
    for q in new_states:
        for c in Sigma:
            temp = temp.union({delta(q, c)})
    new_states = temp.difference(accessible)
    accessible = accessible.union(new_states)
return accessible
```

### 1.2 Indistinguishability Relation ($\equiv$)
Two states $p, q \in Q$ are **indistinguishable** ($p \equiv q$) if for all strings $lpha \in \Sigma^*$:
$$\hat{\delta}(p, lpha) \in F \iff \hat{\delta}(q, lpha) \in F$$
- If there exists $lpha \in \Sigma^*$ such that $\hat{\delta}(p, lpha) \in F \land \hat{\delta}(q, lpha) 
otin F$ (or vice-versa), then $p$ and $q$ are **distinguishable** ($p 
ot\equiv q$).

#### Theorem: $\equiv$ is an Equivalence Relation
1. **Reflexivity:** $q \equiv q$ trivially holds.
2. **Symmetry:** $p \equiv q \iff q \equiv p$.
3. **Transitivity:** $p \equiv q \land q \equiv r \implies p \equiv r$.
4. **Preservation under transitions:** If $p \equiv q$, then for all $a \in \Sigma$, $\delta(p, a) \equiv \delta(q, a)$.

### 1.3 Order-$k$ Indistinguishability ($k\equiv$)
Two states $p, q \in Q$ are $k$-indistinguishable ($p \stackrel{k}{\equiv} q$) if for all $lpha \in \Sigma^*$ with length $|lpha| \le k$:
$$\hat{\delta}(p, lpha) \in F \iff \hat{\delta}(q, lpha) \in F$$

#### Properties of $k$-Indistinguishability:
1. $k\equiv$ is an equivalence relation for every $k \ge 0$.
2. Refinement sequence: $\stackrel{k+1}{\equiv} \; \subseteq \; \stackrel{k}{\equiv}$.
3. Base partition ($0\equiv$): If $F 
eq \emptyset$ and $Q \setminus F 
eq \emptyset$, then $Q / \stackrel{0}{\equiv} = \{F, Q \setminus F\}$.
4. Recursive refinement criterion:
   $$p \stackrel{k+1}{\equiv} q \iff (p \stackrel{k}{\equiv} q) \land orall a \in \Sigma \; (\delta(p, a) \stackrel{k}{\equiv} \delta(q, a))$$
5. Termination condition: If $\stackrel{k+1}{\equiv} \; = \; \stackrel{k}{\equiv}$, then for all $n \ge 0$, $\stackrel{k+n}{\equiv} \; = \; \stackrel{k}{\equiv} \; = \; \equiv$.

---

## 2. Minimal DFA Construction & Moore's Algorithm

### 2.1 Minimal DFA Formal Definition
Given a DFA $M = \langle Q, \Sigma, \delta, q_0, F angle$ with no inaccessible states, the minimal equivalent DFA $M_{min} = \langle Q_{min}, \Sigma, \delta_{min}, q_0^{min}, F_{min} angle$ is defined as:
- $Q_{min} = Q / \equiv$ (the quotient set of equivalence classes $[q]$).
- $\delta_{min}([q], a) = [\delta(q, a)]$.
- $q_0^{min} = [q_0]$.
- $F_{min} = \{[q] \in Q_{min} \mid q \in F\}$.

### 2.2 Uniqueness Theorem
For any regular language $L$, the minimal DFA accepting $L$ is **unique up to state isomorphism**. Any DFA $A'$ accepting $L$ satisfies $|Q_{min}| \le |Q'|$.

### 2.3 Moore's Partition Refinement Algorithm ($O(|Q|^2 \cdot |\Sigma|)$)
```
Input: DFA M = <Q, Sigma, delta, q0, F> without inaccessible states
Output: Partition P = Q / \equiv

P := { F, Q \ F }   (assuming F != empty and Q \ F != empty)
i := 0
repeat
    P_old := P
    Partition each class X in P into sub-blocks based on i-indistinguishability:
        p, q in X remain together iff for all a in Sigma, delta(p, a) and delta(q, a) belong to the same block in P_old.
    P := refined partition
    i := i + 1
until P == P_old
return P
```

---

## 3. Alternative Minimization Algorithms & Complexity Matrix

| Algorithm | Methodological Mechanism | Time Complexity (Worst-Case) | Operational Trade-offs & Notes |
| :--- | :--- | :--- | :--- |
| **Moore (1956)** | Iterative partition refinement of $k$-equivalence classes until stabilization. | $O(|Q|^2 \cdot |\Sigma|)$ | Conceptually direct; standard baseline for manual/pedagogical execution. |
| **Hopcroft (1971)** | Maintains partition $P$ and a worklist $W$ of splitting sets. Always splits by the smaller partition block. | $O(|\Sigma| \cdot |Q| \log |Q|)$ | Most efficient algorithm for large DFAs. Standard implementation choice. |
| **Brzozowski (1963)** | Double reversal and determinization: $M_{min} = ((M^R)_D)^R_D$. | $O(2^{|Q|})$ | Worst-case exponential time, but surprisingly fast on many practical input structures. Automatically prunes unreachable states. |
```
          Brzozowski Workflow:
          M ---> Reverse (M^R) ---> Determinize (M^R_D) ---> Reverse ((M^R_D)^R) ---> Determinize (M_min)
```
