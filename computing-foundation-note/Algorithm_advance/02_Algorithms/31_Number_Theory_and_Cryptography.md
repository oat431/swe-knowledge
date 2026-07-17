---
title: Number Theory and Cryptography
tags:
  - number-theory
  - cryptography
  - rsa
  - clrs
source: CLRS Chapter 31
---

# Number Theory and Cryptography

> Number theory was once viewed as a beautiful but largely useless subject in pure mathematics. Today, number-theoretic algorithms are used widely, due in large part to the invention of cryptographic schemes based on large prime numbers. These schemes are feasible because we can find large primes easily, and they are secure because we do not know how to factor the product of large primes efficiently.

**Input size convention:** We measure input size in terms of the **number of bits** required to represent integers, not just the count of integers. An algorithm with integer inputs $a_1, a_2, \ldots, a_k$ is polynomial-time if it runs in time polynomial in $\lg a_1, \lg a_2, \ldots, \lg a_k$.

---

## 31.1 Elementary Number-Theoretic Notions

### Divisibility

The notation $d \mid a$ ("$d$ divides $a$") means $a = kd$ for some integer $k$. Every integer divides 0. If $d \mid a$ and $d \geq 0$, then $d$ is a **divisor** of $a$.

**Key properties:**
- $d \mid a$ and $d \mid b$ implies $d \mid (a+b)$ and $d \mid (a-b)$
- $d \mid a$ and $d \mid b$ implies $d \mid (ax + by)$ for any integers $x, y$
- $a \mid b$ and $b \mid a$ implies $a = \pm b$

### Primes and Composites

An integer $a > 1$ whose only divisors are 1 and $a$ is **prime**. An integer $a > 1$ that is not prime is **composite**. The integer 1 is a **unit** — neither prime nor composite.

### Division Theorem and Modular Equivalence

**Theorem 31.1 (Division Theorem):** For any integer $a$ and any positive integer $n$, there exist unique integers $q$ and $r$ such that $0 \leq r < n$ and $a = qn + r$.

- $q = \lfloor a/n \rfloor$ is the **quotient**
- $r = a \bmod n$ is the **remainder** (or residue)
- $n \mid a$ if and only if $a \bmod n = 0$

Equivalence class modulo $n$: $[a]_n = \{a + kn : k \in \mathbb{Z}\}$. We write $a \equiv b \pmod{n}$ when $a$ and $b$ are in the same class.

### Greatest Common Divisor

$\gcd(a, b)$ is the largest common divisor of $a$ and $b$ (not both zero). Key properties:

$$\gcd(a, b) = \gcd(b, a), \quad \gcd(a, b) = \gcd(|a|, |b|), \quad \gcd(a, 0) = |a|$$

**Theorem 31.2:** $\gcd(a, b)$ is the smallest positive element of the set $\{ax + by : x, y \in \mathbb{Z}\}$ of linear combinations of $a$ and $b$.

**Corollary 31.5:** For all positive integers $n, a, b$, if $n \mid ab$ and $\gcd(a, n) = 1$, then $n \mid b$.

### Relatively Prime Integers

$a$ and $b$ are **relatively prime** if $\gcd(a, b) = 1$.

**Theorem 31.6:** If $\gcd(a, p) = 1$ and $\gcd(b, p) = 1$, then $\gcd(ab, p) = 1$.

### Unique Factorization

**Theorem 31.7:** If $p$ is prime and $p \mid ab$, then $p \mid a$ or $p \mid b$.

**Theorem 31.8 (Unique Factorization):** Every composite integer $a$ can be written uniquely as:

$$a = p_1^{e_1} p_2^{e_2} \cdots p_r^{e_r}$$

where $p_1 < p_2 < \cdots < p_r$ are primes and $e_i > 0$.

---

## 31.2 Greatest Common Divisor — Euclid's Algorithm

### The GCD Recursion Theorem

**Theorem 31.9:** For any nonnegative integer $a$ and any positive integer $b$:

$$\gcd(a, b) = \gcd(b, a \bmod b)$$

### Euclid's Algorithm

```
EUCLID(a, b):
    if b == 0
        return a
    else return EUCLID(b, a mod b)
```

**Example:** $\gcd(30, 21) = \gcd(21, 9) = \gcd(9, 3) = \gcd(3, 0) = 3$

### Running Time

**Lamé's Theorem (31.11):** If $a > b \geq 1$ and $b < F_{k+1}$, then `EUCLID(a, b)` makes fewer than $k$ recursive calls.

Since $F_k \approx \phi^k / \sqrt{5}$ (where $\phi = (1+\sqrt{5})/2$ is the golden ratio), the number of recursive calls is $O(\lg b)$. For two $\beta$-bit numbers: $O(\beta)$ arithmetic operations, $O(\beta^3)$ bit operations.

### Extended Euclid's Algorithm

Computes not just $d = \gcd(a, b)$ but also coefficients $x, y$ such that:

$$d = \gcd(a, b) = ax + by$$

```
EXTENDED-EUCLID(a, b):
    if b == 0
        return (a, 1, 0)
    else (d', x', y') = EXTENDED-EUCLID(b, a mod b)
         (d, x, y) = (d', y', x' - ⌊a/b⌋ · y')
         return (d, x, y)
```

**Example:** `EXTENDED-EUCLID(99, 78)` returns `(3, -11, 14)`, so $\gcd(99, 78) = 3 = 99 \cdot (-11) + 78 \cdot 14$.

Same running time as `EUCLID`: $O(\lg b)$ recursive calls.

---

## 31.3 Modular Arithmetic

### Groups

A **group** $(S, \oplus)$ has: closure, identity, associativity, and inverses. It is **abelian** if commutative, **finite** if $|S| < \infty$.

### Modular Addition and Multiplication

**Additive group:** $(\mathbb{Z}_n, +_n)$ — size $n$, identity 0, inverse of $a$ is $n - a$.

**Multiplicative group:** $(\mathbb{Z}_n^*, \cdot_n)$ — elements are $\{a \in \mathbb{Z}_n : \gcd(a, n) = 1\}$, identity 1.

$$\mathbb{Z}_n^* = \{[a] \in \mathbb{Z}_n : \gcd(a, n) = 1\}$$

**Theorem 31.13:** $(\mathbb{Z}_n^*, \cdot_n)$ is a finite abelian group. Inverses are computed via `EXTENDED-EUCLID`.

### Euler's Phi Function

$$\phi(n) = |\mathbb{Z}_n^*| = n \prod_{p \text{ prime}, p \mid n} \left(1 - \frac{1}{p}\right)$$

For prime $p$: $\phi(p) = p - 1$. Lower bound for $n > 5$: $\phi(n) > \frac{n}{6 \ln \ln n}$.

### Subgroups and Lagrange's Theorem

**Theorem 31.14:** A nonempty closed subset of a finite group is a subgroup.

**Theorem 31.15 (Lagrange):** If $S_0$ is a subgroup of a finite group $S$, then $|S_0|$ divides $|S|$.

**Corollary 31.16:** If $S_0$ is a proper subgroup of a finite group $S$, then $|S_0| \leq |S|/2$.

### Order of an Element

The **order** of $a$ (denoted $\text{ord}(a)$) is the smallest positive $t$ such that $a^{(t)} = e$. Equivalently, $\text{ord}(a) = |\langle a \rangle|$.

**Corollary 31.19:** For all $a \in S$: $a^{(|S|)} = e$.

---

## 31.4 Solving Modular Linear Equations

Solve $ax \equiv b \pmod{n}$ where $a > 0, n > 0$.

**Theorem 31.20:** Let $d = \gcd(a, n)$. Then $\langle a \rangle = \langle d \rangle = \{0, d, 2d, \ldots, ((n/d)-1)d\}$ in $\mathbb{Z}_n$, and $|\langle a \rangle| = n/d$.

**Corollary 31.21:** $ax \equiv b \pmod{n}$ is solvable $\iff$ $d \mid b$ where $d = \gcd(a, n)$.

**Corollary 31.22:** The equation has either $d$ distinct solutions modulo $n$, or no solutions.

**Theorem 31.23:** If $d = \gcd(a, n)$ and $d \mid b$, then $x_0 = x'_0 \cdot (b/d) \bmod n$ is a solution, where $x'_0$ comes from `EXTENDED-EUCLID(a, n)`.

**Theorem 31.24:** All $d$ solutions are: $x_i = x_0 + i(n/d)$ for $i = 0, 1, \ldots, d-1$.

**Corollary 31.26:** If $\gcd(a, n) = 1$, then $ax \equiv 1 \pmod{n}$ has a **unique** solution. This is the **multiplicative inverse** $a^{-1} \bmod n$, computable via `EXTENDED-EUCLID`.

```
MODULAR-LINEAR-EQUATION-SOLVER(a, b, n):
    (d, x', y') = EXTENDED-EUCLID(a, n)
    if d | b
        x = x' · (b/d) mod n
        for i = 0 to d - 1
            print (x + i·(n/d)) mod n
    else print "no solutions"
```

---

## 31.5 The Chinese Remainder Theorem

**Theorem 31.27 (CRT):** Let $n = n_1 n_2 \cdots n_k$ where the $n_i$ are pairwise relatively prime. The mapping

$$a \leftrightarrow (a_1, a_2, \ldots, a_k) \quad \text{where } a_i = a \bmod n_i$$

is a **bijection** between $\mathbb{Z}_n$ and $\mathbb{Z}_{n_1} \times \mathbb{Z}_{n_2} \times \cdots \times \mathbb{Z}_{n_k}$.

Operations in $\mathbb{Z}_n$ correspond to component-wise operations in the product:
- $(a + b) \bmod n \leftrightarrow ((a_1+b_1) \bmod n_1, \ldots, (a_k+b_k) \bmod n_k)$
- $(ab) \bmod n \leftrightarrow (a_1 b_1 \bmod n_1, \ldots, a_k b_k \bmod n_k)$

**Reconstruction formula:** Define $m_i = n / n_i$ and $c_i = m_i(m_i^{-1} \bmod n_i)$. Then:

$$a = (a_1 c_1 + a_2 c_2 + \cdots + a_k c_k) \bmod n$$

**Corollary 31.28:** The system $x \equiv a_i \pmod{n_i}$ for $i = 1, \ldots, k$ has a **unique** solution modulo $n$.

**Applications:**
1. **Structure theorem:** Describes $\mathbb{Z}_n$ as isomorphic to the Cartesian product
2. **Efficiency:** Working in each $\mathbb{Z}_{n_i}$ separately can be cheaper in bit operations than working modulo $n$ directly

---

## 31.6 Powers of an Element

### Euler's Theorem and Fermat's Theorem

**Theorem 31.30 (Euler):** For any integer $n > 1$: $a^{\phi(n)} \equiv 1 \pmod{n}$ for all $a \in \mathbb{Z}_n^*$.

**Theorem 31.31 (Fermat):** If $p$ is prime: $a^{p-1} \equiv 1 \pmod{p}$ for all $a \in \mathbb{Z}_p^*$.

### Primitive Roots and Discrete Logarithms

If $\text{ord}_n(g) = |\mathbb{Z}_n^*|$, then $g$ is a **primitive root** (generator) of $\mathbb{Z}_n^*$, and the group is **cyclic**.

**Theorem 31.32:** $\mathbb{Z}_n^*$ is cyclic iff $n \in \{2, 4, p^e, 2p^e\}$ for odd prime $p$ and positive integer $e$.

**Discrete logarithm:** If $g^z \equiv a \pmod{n}$, then $z = \text{ind}_{n,g}(a)$.

**Theorem 31.33:** $g^x \equiv g^y \pmod{n} \iff x \equiv y \pmod{\phi(n)}$.

### Nontrivial Square Roots of 1

**Theorem 31.34:** If $p$ is an odd prime and $e \geq 1$, then $x^2 \equiv 1 \pmod{p^e}$ has only two solutions: $x = 1$ and $x = -1$.

**Corollary 31.35:** If there exists a **nontrivial** square root of 1 modulo $n$ (i.e., $x^2 \equiv 1 \pmod{n}$ with $x \not\equiv \pm 1$), then $n$ is **composite**. This is the key to the Miller-Rabin test.

### Modular Exponentiation by Repeated Squaring

```
MODULAR-EXPONENTIATION(a, b, n):
    c = 0
    d = 1
    let ⟨b_k, b_{k-1}, ..., b_0⟩ be the binary representation of b
    for i = k downto 0
        c = 2c
        d = (d · d) mod n
        if b_i == 1
            c = c + 1
            d = (d · a) mod n
    return d
```

**Loop invariant:** Just before each iteration, $c$ equals the prefix $\langle b_k, \ldots, b_{i+1} \rangle$ and $d = a^c \bmod n$.

**Cost:** $O(\beta)$ arithmetic operations, $O(\beta^3)$ bit operations for $\beta$-bit inputs.

**Example:** $7^{560} \bmod 561$: binary $560 = \langle 1000110000 \rangle$, result = 1.

---

## 31.7 The RSA Public-Key Cryptosystem

### Public-Key Cryptography

Each participant has a **public key** $P$ and a **secret key** $S$. The keys specify inverse functions on message space $\mathcal{D}$:

$$M = S_A(P_A(M)) = P_A(S_A(M))$$

- **Encryption:** Bob uses Alice's public key: $C = P_A(M)$
- **Decryption:** Alice uses her secret key: $M = S_A(C)$
- **Digital signature:** Alice signs with secret key: $\sigma = S_A(M')$; Bob verifies with public key: $M' = P_A(\sigma)$

**Security requirement:** Computing $S_A$ from $P_A$ must be computationally infeasible, even though everyone knows $P_A$.

### RSA Setup

1. Select two large random primes $p \neq q$
2. Compute $n = pq$
3. Compute $\phi(n) = (p-1)(q-1)$
4. Select $e$ such that $1 < e < \phi(n)$ and $\gcd(e, \phi(n)) = 1$
5. Compute $d = e^{-1} \bmod \phi(n)$ (via `EXTENDED-EUCLID`)

**Public key:** $P = (e, n)$  
**Secret key:** $S = (d, n)$

### RSA Operations

**Encryption:** $C = M^e \bmod n$  
**Decryption:** $M = C^d \bmod n$

**Correctness (why it works):** Since $ed \equiv 1 \pmod{\phi(n)}$, we have $ed = 1 + k\phi(n)$ for some integer $k$. By Euler's theorem, for $\gcd(M, n) = 1$:

$$C^d \equiv (M^e)^d = M^{ed} = M^{1+k\phi(n)} = M \cdot (M^{\phi(n)})^k \equiv M \cdot 1^k = M \pmod{n}$$

(Also holds when $\gcd(M, n) \neq 1$ by a separate argument using $\text{lcm}(p-1, q-1)$.)

**Efficiency:** Encryption and decryption each require $O(\beta)$ arithmetic operations via `MODULAR-EXPONENTIATION`, where $\beta$ is the bit length of $n$.

### RSA Security

- **Hard problem:** Factoring $n = pq$ into its prime factors
- Finding $d$ from $(e, n)$ is believed equivalent to factoring $n$
- Current best factoring algorithms are sub-exponential but super-polynomial
- Typical key sizes: 2048–4096 bits

### Digital Signatures with RSA

- **Sign:** $\sigma = M^d \bmod n$ (using secret key)
- **Verify:** $M' = \sigma^e \bmod n$ (using public key); accept if $M' = M$

A signed message provides both **authentication** (only the signer knows $d$) and **integrity** (any modification invalidates the signature).

---

## 31.8 Primality Testing

### Miller-Rabin Test

A randomized algorithm that tests whether an odd integer $n$ is prime. Based on the contrapositive of Theorem 31.34: if we find a nontrivial square root of 1 mod $n$, then $n$ is composite.

**Key idea:** Write $n - 1 = 2^t \cdot u$ where $u$ is odd. For a "witness" $a$, compute $a^u, a^{2u}, \ldots, a^{2^t u} \bmod n$. If $n$ is prime, this sequence either:
- Ends in 1 (with all preceding values being 1 or $-1$)
- Never reaches 1

If the sequence reaches 1 after a value other than $\pm 1$, that value is a nontrivial square root of 1, proving $n$ is composite.

**Error probability:** For composite $n$, at least $3/4$ of possible witnesses detect compositeness. After $k$ rounds, the probability of error is $\leq (1/4)^k$.

---

## Summary: Algorithm Complexity

| Problem | Arithmetic Ops | Bit Ops |
|---|---|---|
| $\gcd(a, b)$ | $O(\lg b)$ | $O(\beta^3)$ or $O(\beta^2)$ |
| Extended GCD | $O(\lg b)$ | $O(\beta^3)$ |
| Modular inverse $a^{-1} \bmod n$ | $O(\lg n)$ | $O(\beta^3)$ |
| Modular exponentiation $a^b \bmod n$ | $O(\beta)$ | $O(\beta^3)$ |
| CRT reconstruction | $O(k \lg^2 n)$ | — |
| Miller-Rabin (per round) | $O(\beta)$ | $O(\beta^3)$ |

Where $\beta$ is the bit length of the largest input integer.

## Related

- [[Algorithm Overview]] — Math algorithms (GCD, primes, modular exponentiation)
- [[02 Math Algorithms]] — Basic number theory covered in v1
- [[06_Geometry_NP_and_Approximation]] — Computational complexity of number-theoretic problems


---

## Hands-On Exercises

### Exercise 1: Extended Euclidean Algorithm
Implement `EXTENDED-EUCLID(a, b)` from the note. Trace it for:
- `EXTENDED-EUCLID(99, 78)` → expect `(3, -11, 14)` so `99·(-11) + 78·14 = 3`
- `EXTENDED-EUCLID(35, 15)` → expect `(5, ?)` — find x, y such that `35x + 15y = 5`

```java
long[] extendedGcd(long a, long b) {
    if (b == 0) return new long[]{a, 1, 0};
    long[] prev = extendedGcd(b, a % b);
    long d = prev[0], x = prev[2], y = prev[1] - (a / b) * prev[2];
    return new long[]{d, x, y};
}
```

**Verify:** `d = gcd(a, b)` and `a*x + b*y = d` for both examples.

---

### Exercise 2: Modular Inverse
Using `EXTENDED-EUCLID`, compute:
1. `17⁻¹ mod 43` — find x such that `17x ≡ 1 (mod 43)`
2. `3⁻¹ mod 11` — find x such that `3x ≡ 1 (mod 11)`
3. Verify: `17 · (17⁻¹ mod 43) mod 43 = 1`

```java
long modInverse(long a, long m) {
    long[] result = extendedGcd(a, m);
    if (result[0] != 1) throw new ArithmeticException("No inverse");
    return ((result[1] % m) + m) % m;
}
```

---

### Exercise 3: RSA Encrypt/Decrypt (Small Example)
Given `p = 5, q = 11`:
1. Compute `n = pq` and `φ(n) = (p-1)(q-1)`.
2. Choose `e` such that `gcd(e, φ(n)) = 1`. (Try `e = 3`.)
3. Compute `d = e⁻¹ mod φ(n)`.
4. Encrypt message `M = 9`: compute `C = Mᵉ mod n`.
5. Decrypt: compute `M = Cᵈ mod n`. Verify you get 9 back.

```java
// Step by step:
long p = 5, q = 11;
long n = p * q;        // ?
long phi = (p-1)*(q-1); // ?
long e = 3;             // verify gcd(3, phi) = 1
long d = modInverse(e, phi); // ?
long M = 9;
long C = modPow(M, e, n);    // encrypted
long decrypted = modPow(C, d, n); // should be 9
```

---

### Exercise 4: Miller-Rabin Primality Test
Implement one round of the Miller-Rabin test for `n = 561` (a Carmichael number — composite but passes Fermat's test).

1. Write `n - 1 = 2^t · u` where `u` is odd. For 561: `560 = 2^4 · 35`.
2. Choose witness `a = 2`.
3. Compute `a^u mod n`, `a^{2u} mod n`, ..., `a^{2^t · u} mod n`.
4. Check: does the sequence end in 1? Is there a nontrivial square root of 1?

```java
boolean millerRabin(long n, long a) {
    // Write n-1 = 2^t * u
    long u = n - 1;
    int t = 0;
    while (u % 2 == 0) { u /= 2; t++; }
    // Compute a^u, a^{2u}, ..., a^{2^t * u} mod n
    // Check conditions
}
```

**Test:** `millerRabin(561, 2)` should return `false` (561 is composite).

---

### Exercise 5: Chinese Remainder Theorem
Solve the system:
```
x ≡ 2 (mod 3)
x ≡ 3 (mod 5)
x ≡ 2 (mod 7)
```

1. Verify the moduli are pairwise coprime.
2. Compute `n = 3 · 5 · 7 = 105`.
3. For each equation, compute `mᵢ = n/nᵢ` and `cᵢ = mᵢ · (mᵢ⁻¹ mod nᵢ)`.
4. Reconstruct: `x = (a₁c₁ + a₂c₂ + a₃c₃) mod n`.
5. Verify your answer satisfies all three equations.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Count Primes](https://leetcode.com/problems/count-primes/) (LC 204) | 🟡 Medium | Sieve of Eratosthenes |
| 2 | [Pow(x, n)](https://leetcode.com/problems/powx-n/) (LC 50) | 🟡 Medium | Modular exponentiation |
| 3 | [Ugly Number III](https://leetcode.com/problems/ugly-number-iii/) (LC 1201) | 🟡 Medium | GCD/LCM + Inclusion-Exclusion |
| 4 | [The kth Factor of n](https://leetcode.com/problems/the-kth-factor-of-n/) (LC 1492) | 🟡 Medium | Divisibility |
| 5 | [Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/) (LC 365) | 🟡 Medium | Bézout's identity (GCD) |
| 6 | **Implement RSA (full key gen + encrypt/decrypt)** | 🔴 Code | RSA pipeline |
| 7 | **Implement Miller-Rabin Test** | 🔴 Code | Primality testing |
| 8 | **Prove: if p is prime, a^{p-1} ≡ 1 (mod p)** | 🔴 Theory | Fermat's Little Theorem |

### Assignment Guidelines
- **Start** with 1–5 (LeetCode) — these apply GCD, modular arithmetic, and number theory concepts.
- **Problem 5** (Water and Jug) is a beautiful application of Bézout's identity: you can measure `z` liters iff `z` is a multiple of `gcd(x, y)`.
- **Problems 6–7** are coding projects — implement the full RSA pipeline and Miller-Rabin test.
- **Problem 8** is a proof exercise — use Lagrange's theorem on the group `Z*_p`.
- **Target time:** 15 min per Medium, 45 min for RSA/Miller-Rabin projects.
