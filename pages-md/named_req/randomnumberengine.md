# C++ named requirements: RandomNumberEngine (since C++11)

A random number engine is a function object returning unsigned integer values
such that each value in the range of possible results has (ideally) equal
probability.

Any random number engine is also a UniformRandomBitGenerator, and therefore may
be plugged into any random number distribution in order to obtain a random
number (formally, a random variate).

### Requirements

A type `E` satisfying UniformRandomBitGenerator will additionally satisfy
RandomNumberEngine if, given

- `T`, the type named by `E::result_type`
- `s`, a value of type `T`
- `e`, a non-const value of type `E`
- `v`, an lvalue of type `E`
- `x` and `y`, possibly const values of type `E`
- `q`, an lvalue of some type satisfying SeedSequence
- `z`, a value of type `unsigned long long`
- `os`, an output stream
- `is`, an input stream

the following expressions are valid and have their specified effects:

  Expression | Return type | Requirements
  `E()` | Creates an engine with the same state as all other default-constructed
      engines of type `E`.
  `E(x)` | Creates an engine with the same state as `x`.
  `E(s)` | Creates an engine whose initial state is determined by the integer
      `s`.
  `E(q)` | Creates an engine whose initial state is determined by a single call
      to `q.generate`.
  `e.seed()` | `void` | Sets `e == E()`.
  `e.seed(s)` | `void` | Sets `e == E(s)`.
  `e.seed(q)` | `void` | Sets `e == E(q)`.
  `e()` | `T` | Returns a value in the closed interval `[E::min(), E::max()]`.
      Has amortized constant complexity.
  `e.discard(z)` | `void` | Advances `e`'s state as if by `z` consecutive calls
      to `e()`.
  `x == y` | `bool` | `true` if `x` and `y` are in the same state (such that
      repeated future calls to `x()` and `y()` will produce identical
      sequences). Otherwise, `false`.
  `x != y` | `bool` | `!(x == y)`
  `os << x` | `decltype(os)&` | Writes to `os` the textual representation of
      `x`'s current state. In the output, adjacent numbers are separated by one
      or more space characters. If `os`'s fmtflags are not set to
      `ios_base::dec|ios_base::left`, the behavior may be undefined.
  `is >> v` | `decltype(is)&` | Reads from `is` the textual representation of
      `v`'s current state, such that if that state was previously written via
      `os << x`, then `x == v`. If `is`'s fmtflags are not set to
      `ios_base::dec`, the behavior may be undefined.

### Standard library

The following standard library facilities satisfy RandomNumberEngine:

- **linear_congruential_engine (C++11)** — implements linear congruential
  algorithm (class template)
- **mersenne_twister_engine (C++11)** — implements Mersenne twister algorithm
  (class template)
- **subtract_with_carry_engine (C++11)** — implements a subtract-with-carry
  (lagged Fibonacci) algorithm (class template)
- **discard_block_engine (C++11)** — discards some output of a random number
  engine (class template)
- **independent_bits_engine (C++11)** — packs the output of a random number
  engine into blocks of a specified number of bits (class template)
- **shuffle_order_engine (C++11)** — delivers the output of a random number
  engine in a different order (class template)

The following standard library facilities satisfy UniformRandomBitGenerator but
not RandomNumberEngine:

- **random_device (C++11)** — non-deterministic random number generator using
  hardware entropy source (class)

---
*Source: https://en.cppreference.com/w/cpp/named_req/RandomNumberEngine*
