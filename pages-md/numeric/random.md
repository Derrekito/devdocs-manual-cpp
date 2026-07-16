# Pseudo-random number generation

The random number library provides classes that generate random and
pseudo-random numbers. These classes include:

- Uniform random bit generators (URBGs), which include both random number
  engines, which are pseudo-random number generators that generate integer
  sequences with a uniform distribution, and true random number generators if
  available;
- Random number distributions (e.g. uniform, normal, or poisson distributions)
  which convert the output of URBGs into various statistical distributions.

URBGs and distributions are designed to be used together to produce random
values. All of the random number engines may be specifically seeded, serialized,
and de-serialized for use with repeatable simulators.

### Uniform random bit generators

A *uniform random bit generator* is a function object returning unsigned integer
values such that each value in the range of possible results has (ideally) equal
probability of being returned.

All uniform random bit generators meet the UniformRandomBitGenerator
requirements. C++20 also defines a `uniform_random_bit_generator` concept.

- **uniform_random_bit_generator (C++20)** — specifies that a type qualifies as
  a uniform random bit generator (concept)

#### Random number engines

Random number engines generate pseudo-random numbers using seed data as entropy
source. Several different classes of pseudo-random number generation algorithms
are implemented as templates that can be customized.

The choice of which engine to use involves a number of trade-offs: the linear
congruential engine is moderately fast and has a very small storage requirement
for state. The lagged Fibonacci generators are very fast even on processors
without advanced arithmetic instruction sets, at the expense of greater state
storage and sometimes less desirable spectral characteristics. The Mersenne
twister is slower and has greater state storage requirements but with the right
parameters has the longest non-repeating sequence with the most desirable
spectral characteristics (for a given definition of desirable).

None of the random number engines provided by the standard library are
cryptographically secure, as with any secure operation, a crypto library should
be used for the purpose (e.g. OpenSSL `RAND_bytes`).

- **linear_congruential_engine (C++11)** — implements linear congruential
  algorithm (class template)
- **mersenne_twister_engine (C++11)** — implements Mersenne twister algorithm
  (class template)
- **subtract_with_carry_engine (C++11)** — implements a subtract-with-carry
  (lagged Fibonacci) algorithm (class template)

#### Random number engine adaptors

Random number engine adaptors generate pseudo-random numbers using another
random number engine as entropy source. They are generally used to alter the
spectral characteristics of the underlying engine.

- **discard_block_engine (C++11)** — discards some output of a random number
  engine (class template)
- **independent_bits_engine (C++11)** — packs the output of a random number
  engine into blocks of a specified number of bits (class template)
- **shuffle_order_engine (C++11)** — delivers the output of a random number
  engine in a different order (class template)

#### Predefined random number generators

Several specific popular algorithms are predefined.

- **`minstd_rand0` (C++11)** —
  `std::linear_congruential_engine<std::uint_fast32_t, 16807, 0, 2147483647>`
  Discovered in 1969 by Lewis, Goodman and Miller, adopted as "Minimal standard"
  in 1988 by Park and Miller
- **`minstd_rand` (C++11)** —
  `std::linear_congruential_engine<std::uint_fast32_t, 48271, 0, 2147483647>`
  Newer "Minimum standard", recommended by Park, Miller, and Stockmeyer in 1993
- **`mt19937`(C++11)** — `std::mersenne_twister_engine<std::uint_fast32_t, 32,
  624, 397, 31, 0x9908b0df, 11, 0xffffffff, 7, 0x9d2c5680, 15, 0xefc60000, 18,
  1812433253>` 32-bit Mersenne Twister by Matsumoto and Nishimura, 1998
- **`mt19937_64`(C++11)** — `std::mersenne_twister_engine<std::uint_fast64_t,
  64, 312, 156, 31, 0xb5026f5aa96619e9, 29, 0x5555555555555555, 17,
  0x71d67fffeda60000, 37, 0xfff7eee000000000, 43, 6364136223846793005>` 64-bit
  Mersenne Twister by Matsumoto and Nishimura, 2000
- **`ranlux24_base` (C++11)** —
  `std::subtract_with_carry_engine<std::uint_fast32_t, 24, 10, 24>`
- **`ranlux48_base` (C++11)** —
  `std::subtract_with_carry_engine<std::uint_fast64_t, 48, 5, 12>`
- **`ranlux24`(C++11)** — `std::discard_block_engine<std::ranlux24_base, 223,
  23>` 24-bit RANLUX generator by Martin Lüscher and Fred James, 1994
- **`ranlux48`(C++11)** — `std::discard_block_engine<std::ranlux48_base, 389,
  11>` 48-bit RANLUX generator by Martin Lüscher and Fred James, 1994
- **`knuth_b` (C++11)** — `std::shuffle_order_engine<std::minstd_rand0, 256>`
- **`default_random_engine`(C++11)** — *implementation-defined*

#### Non-deterministic random numbers

`std::random_device` is a non-deterministic uniform random bit generator,
although implementations are allowed to implement `std::random_device` using a
pseudo-random number engine if there is no support for non-deterministic random
number generation.

- **random_device (C++11)** — non-deterministic random number generator using
  hardware entropy source (class)

### Random number distributions

A random number distribution post-processes the output of a URBG in such a way
that resulting output is distributed according to a defined statistical
probability density function.

Random number distributions satisfy RandomNumberDistribution.

**Uniform distributions**

- **uniform_int_distribution (C++11)** — produces integer values evenly
  distributed across a range (class template)
- **uniform_real_distribution (C++11)** — produces real values evenly
  distributed across a range (class template)

**Bernoulli distributions**

- **bernoulli_distribution (C++11)** — produces `bool` values on a Bernoulli
  distribution (class)
- **binomial_distribution (C++11)** — produces integer values on a binomial
  distribution (class template)
- **negative_binomial_distribution (C++11)** — produces integer values on a
  negative binomial distribution (class template)
- **geometric_distribution (C++11)** — produces integer values on a geometric
  distribution (class template)

**Poisson distributions**

- **poisson_distribution (C++11)** — produces integer values on a Poisson
  distribution (class template)
- **exponential_distribution (C++11)** — produces real values on an exponential
  distribution (class template)
- **gamma_distribution (C++11)** — produces real values on a gamma distribution
  (class template)
- **weibull_distribution (C++11)** — produces real values on a Weibull
  distribution (class template)
- **extreme_value_distribution (C++11)** — produces real values on an extreme
  value distribution (class template)

**Normal distributions**

- **normal_distribution (C++11)** — produces real values on a standard normal
  (Gaussian) distribution (class template)
- **lognormal_distribution (C++11)** — produces real values on a lognormal
  distribution (class template)
- **chi_squared_distribution (C++11)** — produces real values on a chi-squared
  distribution (class template)
- **cauchy_distribution (C++11)** — produces real values on a Cauchy
  distribution (class template)
- **fisher_f_distribution (C++11)** — produces real values on a Fisher's
  F-distribution (class template)
- **student_t_distribution (C++11)** — produces real values on a Student's
  t-distribution (class template)

**Sampling distributions**

- **discrete_distribution (C++11)** — produces random integers on a discrete
  distribution (class template)
- **piecewise_constant_distribution (C++11)** — produces real values distributed
  on constant subintervals (class template)
- **piecewise_linear_distribution (C++11)** — produces real values distributed
  on defined subintervals (class template)

### Utilities

- **generate_canonical (C++11)** — evenly distributes real values of given
  precision across `[``​0​``,``1``)` (function template)
- **seed_seq (C++11)** — general-purpose bias-eliminating scrambled seed
  sequence generator (class)

### C random library

In addition to the engines and distributions described above, the functions and
constants from the C random library are also available though not recommended:

- **rand** — generates a pseudo-random number (function)
- **srand** — seeds pseudo-random number generator (function)
- **RAND_MAX** — maximum possible value generated by `std::rand` (macro
  constant)

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
#include <map>
#include <random>
#include <string>

int main()
{
    // Seed with a real random value, if available
    std::random_device r;

    // Choose a random mean between 1 and 6
    std::default_random_engine e1(r());
    std::uniform_int_distribution<int> uniform_dist(1, 6);
    int mean = uniform_dist(e1);
    std::cout << "Randomly-chosen mean: " << mean << '\n';

    // Generate a normal distribution around that mean
    std::seed_seq seed2{r(), r(), r(), r(), r(), r(), r(), r()};
    std::mt19937 e2(seed2);
    std::normal_distribution<> normal_dist(mean, 2);

    std::map<int, int> hist;
    for (int n = 0; n != 10000; ++n)
        ++hist[std::round(normal_dist(e2))];

    std::cout << "Normal distribution around " << mean << ":\n"
              << std::fixed << std::setprecision(1);
    for (auto [x, y] : hist)
        std::cout << std::setw(2) << x << ' ' << std::string(y / 200, '*') << '\n';
}
```

Possible output:

```text
Randomly-chosen mean: 4
Normal distribution around 4:
-4
-3
-2
-1
 0 *
 1 ***
 2 ******
 3 ********
 4 *********
 5 ********
 6 ******
 7 ***
 8 *
 9
10
11
12
```

### See also

**C documentation for Pseudo-random number generation**

---
*Source: https://en.cppreference.com/w/cpp/numeric/random*
