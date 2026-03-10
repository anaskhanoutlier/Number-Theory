

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
from math import sqrt, log
import time

# ─────────────────────────────────────────
# SECTION 1: PRIME NUMBER ALGORITHMS
# ─────────────────────────────────────────

def sieve_of_eratosthenes(limit):
    """
    Classic Sieve of Eratosthenes algorithm.
    Time Complexity: O(n log log n)
    Returns all prime numbers up to `limit`.
    """
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False

    for i in range(2, int(sqrt(limit)) + 1):
        if is_prime[i]:
            for j in range(i * i, limit + 1, i):
                is_prime[j] = False

    return [i for i in range(2, limit + 1) if is_prime[i]]


def is_prime_trial_division(n):
    """Trial division method for single number primality test."""
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(sqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    return True


def prime_factorization(n):
    """Returns prime factorization of n as a dictionary {prime: exponent}."""
    factors = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors[d] = factors.get(d, 0) + 1
            n //= d
        d += 1
    if n > 1:
        factors[n] = factors.get(n, 0) + 1
    return factors


def goldbach_pairs(n):
    """Goldbach's Conjecture: Every even integer > 2 is sum of two primes."""
    if n <= 2 or n % 2 != 0:
        return []
    primes_set = set(sieve_of_eratosthenes(n))
    pairs = []
    for p in primes_set:
        if (n - p) in primes_set and p <= n - p:
            pairs.append((p, n - p))
    return pairs


# ─────────────────────────────────────────
# SECTION 2: MATHEMATICAL SERIES
# ─────────────────────────────────────────

def harmonic_series(n_terms):
    """Harmonic Series: H_n = 1 + 1/2 + 1/3 + ... + 1/n"""
    terms = np.array([1/k for k in range(1, n_terms + 1)])
    cumulative = np.cumsum(terms)
    return terms, cumulative


def basel_problem_series(n_terms):
    """Basel Problem: sum(1/n^2) = pi^2/6  (Euler, 1734)"""
    n = np.arange(1, n_terms + 1)
    terms = 1 / n**2
    cumulative = np.cumsum(terms)
    true_value = np.pi**2 / 6
    error = np.abs(cumulative - true_value)
    return terms, cumulative, true_value, error


def prime_counting_function(limit):
    """
    pi(x) = number of primes <= x  
    Compared against Prime Number Theorem approximation: x / ln(x)
    """
    primes = sieve_of_eratosthenes(limit)
    x_vals = np.arange(2, limit + 1)
    prime_set = set(primes)
    
    count = 0
    pi_x = []
    for x in x_vals:
        if x in prime_set:
            count += 1
        pi_x.append(count)

    # Prime Number Theorem approximation
    pnt_approx = x_vals / np.log(x_vals)
    return x_vals, np.array(pi_x), pnt_approx


# ─────────────────────────────────────────
# SECTION 3: VISUALIZATION
# ─────────────────────────────────────────

def plot_all_results():
    """Comprehensive visualization of all number theory concepts."""
    fig = plt.figure(figsize=(16, 12))
    fig.suptitle("Prime Number Theory & Mathematical Series\nBSc Mathematics Project | Python + NumPy + Matplotlib",
                 fontsize=14, fontweight='bold', y=0.98)
    
    gs = gridspec.GridSpec(2, 3, figure=fig, hspace=0.45, wspace=0.35)

    # ── Plot 1: Prime Distribution (Ulam Spiral) ──
    ax1 = fig.add_subplot(gs[0, 0])
    size = 31
    spiral = np.zeros((size, size), dtype=int)
    primes_set = set(sieve_of_eratosthenes(size * size))

    x, y = size // 2, size // 2
    dx, dy = 0, -1
    for i in range(1, size * size + 1):
        if 0 <= x < size and 0 <= y < size:
            spiral[y][x] = 1 if i in primes_set else 0
        if x == y or (x < y and x + y == size - 1) or (x > y and x + y == size):
            dx, dy = -dy, dx
        x, y = x + dx, y + dy

    ax1.imshow(spiral, cmap='Blues', interpolation='nearest')
    ax1.set_title("Ulam Spiral\n(Blue = Prime)", fontsize=9, fontweight='bold')
    ax1.axis('off')

    # ── Plot 2: Prime Number Theorem ──
    ax2 = fig.add_subplot(gs[0, 1])
    limit = 500
    x_vals, pi_x, pnt = prime_counting_function(limit)
    ax2.plot(x_vals, pi_x, label='π(x) actual', color='royalblue', linewidth=2)
    ax2.plot(x_vals, pnt, label='x/ln(x) PNT', color='tomato', linewidth=1.5, linestyle='--')
    ax2.set_xlabel('x')
    ax2.set_ylabel('Count')
    ax2.set_title("Prime Counting π(x)\nvs PNT Approx", fontsize=9, fontweight='bold')
    ax2.legend(fontsize=7)
    ax2.grid(True, alpha=0.3)

    # ── Plot 3: Prime Gaps ──
    ax3 = fig.add_subplot(gs[0, 2])
    primes_list = sieve_of_eratosthenes(500)
    gaps = [primes_list[i+1] - primes_list[i] for i in range(len(primes_list)-1)]
    ax3.bar(range(len(gaps[:60])), gaps[:60], color='steelblue', edgecolor='navy', linewidth=0.5)
    ax3.set_xlabel('Index')
    ax3.set_ylabel('Gap Size')
    ax3.set_title("Prime Gaps\n(First 60 Gaps)", fontsize=9, fontweight='bold')
    ax3.grid(True, alpha=0.3, axis='y')

    # ── Plot 4: Basel Problem Convergence ──
    ax4 = fig.add_subplot(gs[1, 0])
    terms, cumsum, true_val, error = basel_problem_series(200)
    ax4.plot(range(1, 201), cumsum, color='purple', linewidth=2, label='Partial Sum')
    ax4.axhline(y=true_val, color='red', linestyle='--', linewidth=1.5, label=f'π²/6 ≈ {true_val:.4f}')
    ax4.set_xlabel('Number of Terms')
    ax4.set_ylabel('Sum')
    ax4.set_title("Basel Problem\nΣ(1/n²) → π²/6", fontsize=9, fontweight='bold')
    ax4.legend(fontsize=7)
    ax4.grid(True, alpha=0.3)

    # ── Plot 5: Harmonic Series (Divergence) ──
    ax5 = fig.add_subplot(gs[1, 1])
    _, harmonic_cumsum = harmonic_series(500)
    n_range = np.arange(1, 501)
    ax5.plot(n_range, harmonic_cumsum, color='darkgreen', linewidth=2, label='H_n (actual)')
    ax5.plot(n_range, np.log(n_range), color='orange', linewidth=1.5, linestyle='--', label='ln(n) approx')
    ax5.set_xlabel('n')
    ax5.set_ylabel('H_n')
    ax5.set_title("Harmonic Series Divergence\nH_n ≈ ln(n) + γ", fontsize=9, fontweight='bold')
    ax5.legend(fontsize=7)
    ax5.grid(True, alpha=0.3)

    # ── Plot 6: Goldbach Verification ──
    ax6 = fig.add_subplot(gs[1, 2])
    even_numbers = range(4, 102, 2)
    min_prime_pairs = []
    for num in even_numbers:
        pairs = goldbach_pairs(num)
        if pairs:
            min_prime_pairs.append(pairs[0][0])
        else:
            min_prime_pairs.append(0)
    ax6.bar(range(len(min_prime_pairs)), min_prime_pairs, color='coral', edgecolor='darkred', linewidth=0.5)
    ax6.set_xlabel('Even Number Index (4, 6, 8...)')
    ax6.set_ylabel('Smaller Prime in Pair')
    ax6.set_title("Goldbach Conjecture\nSmallest Prime in Each Pair", fontsize=9, fontweight='bold')
    ax6.grid(True, alpha=0.3, axis='y')

    plt.savefig("project1_prime_number_theory.png", dpi=150, bbox_inches='tight')
    plt.show()
    print("✅ Plot saved as 'project1_prime_number_theory.png'")


# ─────────────────────────────────────────
# SECTION 4: MAIN ANALYSIS & REPORT
# ─────────────────────────────────────────

def main():
    print("=" * 60)
    print("PROJECT 1: Prime Number Theory & Mathematical Series")
    print("BSc Mathematics | IGNOU | Python + NumPy + Matplotlib")
    print("=" * 60)

    # Primes up to 100
    primes_100 = sieve_of_eratosthenes(100)
    print(f"\n📌 Primes up to 100 ({len(primes_100)} total):")
    print(primes_100)

    # Prime factorization examples
    print("\n📌 Prime Factorizations:")
    for n in [360, 1024, 999, 2310]:
        factors = prime_factorization(n)
        factor_str = " × ".join([f"{p}^{e}" if e > 1 else str(p) for p, e in factors.items()])
        print(f"  {n} = {factor_str}")

    # Basel Problem
    _, cumsum, true_val, error = basel_problem_series(1000)
    print(f"\n📌 Basel Problem (1000 terms):")
    print(f"  Partial Sum  = {cumsum[-1]:.8f}")
    print(f"  True (π²/6)  = {true_val:.8f}")
    print(f"  Error        = {error[-1]:.2e}")

    # Harmonic Series
    _, h_cumsum = harmonic_series(1000)
    print(f"\n📌 Harmonic Series H_1000 = {h_cumsum[-1]:.6f}  (diverges slowly)")

    # Goldbach Pairs for selected even numbers
    print("\n📌 Goldbach Conjecture Verification:")
    for n in [28, 50, 100, 200]:
        pairs = goldbach_pairs(n)
        print(f"  {n} = {pairs[0][0]} + {pairs[0][1]}  (and {len(pairs)-1} other pair(s))")

    # Timing comparison
    print("\n📌 Algorithm Performance:")
    start = time.time()
    sieve_of_eratosthenes(100000)
    print(f"  Sieve (n=100,000): {(time.time()-start)*1000:.2f} ms")

    print("\n📊 Generating visualizations...")
    plot_all_results()


if __name__ == "__main__":
    main()
