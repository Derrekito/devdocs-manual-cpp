# C++ named requirements: UniformRandomBitGenerator (since C++11)

A uniform random bit generator is a function object returning unsigned integer
values such that each value in the range of possible results has (ideally) equal
probability.

Uniform random bit generators are not intended to be used as random number
generators: they are used as the source of random bits (generated in bulk, for
efficiency). Any uniform random bit generator may be plugged into any random
number distribution in order to obtain a random number (formally, a random
variate).

### Requirements

The type `G` satisfies UniformRandomBitGenerator if

Given `g`, a value of type `G`, all following conditions are satisfied:
- `G::result_type` is valid, and denotes an unsigned integer type.
- The following expressions must be valid and have their specified effects:
*(until C++20)*

  Expression | Type | Requirements
  `G::min()` | `G::result_type` | Returns the smallest value that `G`'s
      operator() may return. The return value is strictly less than `G::max()`.
      The function must be constexpr.
  `G::max()` | `G::result_type` | Returns the largest value that `G`'s
      operator() may return. The return value is strictly greater than
      `G::min()`. The function must be constexpr.
  `g()` | `G::result_type` | Returns a value in the closed interval
      `[``G::min()``,``G::max()``]`. Has amortized constant complexity.

All following conditions are satisfied:
- `G` models `uniform_random_bit_generator`.
- std::invoke_result_t<G&> is an unsigned integer type.
- `G` provides a nested typedef name `result_type`, which denotes the same type
  as std::invoke_result_t<G&>.
*(since C++20)*

### Notes

All RandomNumberEngines satisfy this requirement.

### Standard library

The following standard library facilities expect a UniformRandomBitGenerator
type.

- **random_shuffleshuffle (until C++17)(C++11)** — randomly re-orders elements
  in a range (function template)
- **sample (C++17)** — selects N random elements from a sequence (function
  template)
- **generate_canonical (C++11)** — evenly distributes real values of given
  precision across `[``​0​``,``1``)` (function template)
- **uniform_int_distribution (C++11)** — produces integer values evenly
  distributed across a range (class template)
- **uniform_real_distribution (C++11)** — produces real values evenly
  distributed across a range (class template)

**all other random number distributions**

The following standard library facilities satisfy UniformRandomBitGenerator
without additionally satisfying RandomNumberEngine:

- **random_device (C++11)** — non-deterministic random number generator using
  hardware entropy source (class)

### See also

- **uniform_random_bit_generator (C++20)** — specifies that a type qualifies as
  a uniform random bit generator (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/UniformRandomBitGenerator*
