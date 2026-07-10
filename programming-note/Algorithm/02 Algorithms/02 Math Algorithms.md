---
tags:
- algorithms
- math
- number-theory
- programming
---

# 02 Math Algorithms

Computational math algorithms are the "how to compute" companion to mathematical theory. You need GCD for fractions, sieves for primes, and modular arithmetic for cryptography and combinatorics. These are the building blocks that make number-heavy problems tractable.

---

## GCD / LCM

### Euclidean Algorithm for GCD

The greatest common divisor of two numbers can be computed in **O(log(min(a, b)))** using repeated modulo operations.

```java
// Iterative GCD — O(log(min(a, b)))
long gcd(long a, long b) {
    while (b != 0) {
        long temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// Recursive version
long gcd(long a, long b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

### LCM from GCD

```
lcm(a, b) = a / gcd(a, b) * b
```

Always divide before multiplying to avoid overflow.

```java
long lcm(long a, long b) {
    return a / gcd(a, b) * b;
}
```

| Operation | Time | Notes |
|-----------|:----:|-------|
| `gcd(a, b)` | O(log(min(a,b))) | Euclidean algorithm |
| `lcm(a, b)` | O(log(min(a,b))) | Uses gcd internally |
| `gcd` of array | O(n · log(max)) | Fold: `gcd(gcd(a,b), c)` |

---

## Sieve of Eratosthenes

Find **all primes up to n** in O(n log log n). Much faster than checking each number individually with trial division.

### How It Works

1. Create boolean array `isPrime[0..n]`, initialize all to `true`
2. Mark 0 and 1 as `false`
3. For each number `i` from 2 to √n: if `isPrime[i]` is `true`, mark all multiples of `i` as `false`
4. Remaining `true` entries are primes

### Full Implementation (Optimized — Skip Evens)

```java
// Sieve of Eratosthenes — O(n log log n) time, O(n) space
boolean[] sieve(int n) {
    boolean[] isPrime = new boolean[n + 1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    
    // Handle 2 separately, then skip all even numbers
    for (int i = 4; i <= n; i += 2) {
        isPrime[i] = false;
    }
    
    // Only check odd numbers starting from 3
    for (int i = 3; i * i <= n; i += 2) {
        if (isPrime[i]) {
            // Start marking from i*i (smaller multiples already handled)
            for (int j = i * i; j <= n; j += 2 * i) {
                isPrime[j] = false;
            }
        }
    }
    return isPrime;
}

// Get list of primes from sieve
List<Integer> getPrimes(int n) {
    boolean[] isPrime = sieve(n);
    List<Integer> primes = new ArrayList<>();
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) primes.add(i);
    }
    return primes;
}
```

### Sieve vs Trial Division

| | Sieve | Trial Division |
|---|:---:|:---:|
| **Find all primes ≤ n** | O(n log log n) | O(n√n) |
| **Check if one number is prime** | Overkill | O(√n) |
| **Memory** | O(n) | O(1) |
| **Best for** | Many primes | Single primality test |

---

## Modular Exponentiation (Fast Pow)

Computing `base^exp % mod` naively takes O(exp) multiplications. **Fast pow** uses the binary representation of the exponent to compute it in **O(log exp)**.

### Key Idea

```
base^13 = base^(1101₂) = base^8 × base^4 × base^1
```

Walk through bits of the exponent. Square the base at each step; multiply into result when the bit is 1.

### Full Implementation (Iterative)

```java
// Fast modular exponentiation — O(log exp)
long modPow(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    
    while (exp > 0) {
        if ((exp & 1) == 1) {         // If current bit is 1
            result = result * base % mod;
        }
        base = base * base % mod;     // Square the base
        exp >>= 1;                    // Move to next bit
    }
    return result;
}
```

**Walkthrough:** `3^13 % 7`

| Step | exp (binary) | Bit | base | result |
|------|:------------:|:---:|:----:|:------:|
| 0 | 1101 | 1 | 3 | 3 |
| 1 | 110 | 0 | 3²=2 | 3 |
| 2 | 11 | 1 | 2²=4 | 3×4=5 |
| 3 | 1 | 1 | 4²=2 | 5×2=3 |

Result: `3^13 % 7 = 3`

| Operation | Naive | Fast Pow |
|-----------|:-----:|:--------:|
| `base^exp % mod` | O(exp) | **O(log exp)** |

---

## Modular Inverse

The modular inverse of `a` modulo `m` is a number `a⁻¹` such that:

```
a × a⁻¹ ≡ 1 (mod m)
```

It exists only when `gcd(a, m) = 1` (a and m are coprime).

### Method 1: Fermat's Little Theorem (when m is prime)

If `m` is prime: `a⁻¹ ≡ a^(m-2) (mod m)`

```java
// Modular inverse using Fermat's Little Theorem — O(log m)
// Only works when m is prime
long modInverseFermat(long a, long m) {
    return modPow(a, m - 2, m);
}
```

### Method 2: Extended Euclidean Algorithm (general case)

Finds `x` such that `a * x + m * y = gcd(a, m) = 1`. Then `x` is the modular inverse.

```java
// Extended Euclidean Algorithm — O(log(min(a, m)))
// Returns {gcd, x, y} where a*x + m*y = gcd
long[] extendedGcd(long a, long b) {
    if (b == 0) return new long[]{a, 1, 0};
    long[] prev = extendedGcd(b, a % b);
    long g = prev[0], x = prev[2], y = prev[1] - (a / b) * prev[2];
    return new long[]{g, x, y};
}

// Modular inverse using Extended GCD — works for any coprime a, m
long modInverse(long a, long m) {
    long[] result = extendedGcd(a, m);
    if (result[0] != 1) throw new ArithmeticException("Inverse doesn't exist");
    return ((result[1] % m) + m) % m;  // Ensure positive
}
```

| Method | Requirement | Time |
|--------|-------------|:----:|
| Fermat's Little Theorem | m must be prime | O(log m) |
| Extended Euclidean | gcd(a, m) = 1 | O(log(min(a,m))) |

---

## Combinatorics under Mod

Computing `nCr % p` (binomial coefficient modulo a prime) is essential for competitive programming and combinatorics problems.

### nCr using Modular Inverse

```
nCr = n! / (r! × (n - r)!)
nCr % p = n! × (r!)⁻¹ × ((n-r)!)⁻¹  mod p
```

### Precompute Factorials + Inverse Factorials

```java
// Precompute factorials and inverse factorials modulo p — O(n)
class Combinatorics {
    static final long MOD = 1_000_000_007;
    long[] fact;      // fact[i] = i! % MOD
    long[] invFact;   // invFact[i] = (i!)^(-1) % MOD

    Combinatorics(int n) {
        fact = new long[n + 1];
        invFact = new long[n + 1];
        fact[0] = 1;
        for (int i = 1; i <= n; i++) {
            fact[i] = fact[i - 1] * i % MOD;
        }
        invFact[n] = modPow(fact[n], MOD - 2, MOD);  // Fermat
        for (int i = n - 1; i >= 0; i--) {
            invFact[i] = invFact[i + 1] * (i + 1) % MOD;
        }
    }

    // nCr % MOD in O(1) after O(n) precomputation
    long nCr(int n, int r) {
        if (r < 0 || r > n) return 0;
        return fact[n] % MOD * invFact[r] % MOD * invFact[n - r] % MOD;
    }

    // nPr % MOD (permutations)
    long nPr(int n, int r) {
        if (r < 0 || r > n) return 0;
        return fact[n] % MOD * invFact[n - r] % MOD;
    }
}
```

| Operation | Precomputation | Per Query |
|-----------|:--------------:|:---------:|
| `nCr % p` | O(n) | **O(1)** |
| `nPr % p` | O(n) | **O(1)** |

---

## When to Use What

| Problem | Algorithm | Time |
|---------|-----------|:----:|
| Find GCD of two numbers | Euclidean | O(log(min(a,b))) |
| Find all primes ≤ n | Sieve of Eratosthenes | O(n log log n) |
| Check if n is prime | Trial division | O(√n) |
| Compute a^b % m | Fast pow | O(log b) |
| Find a⁻¹ mod m (m prime) | Fermat's Little Theorem | O(log m) |
| Find a⁻¹ mod m (general) | Extended Euclidean | O(log(min(a,m))) |
| nCr % p (many queries) | Precompute factorials | O(n) + O(1)/query |

> For mathematical theory behind these algorithms, see [[Number Theory]] and [[Basics of Counting]] in the Math vault.

---

## Sources

- CLRS — Chapter 31 (Number-Theoretic Algorithms)
- Competitive Programming 3 — Steven Halim, Chapters 5-7
- GeeksforGeeks — Modular Arithmetic
