# std::negative_binomial_distribution<IntType>::negative_binomial_distribution

```cpp
negative_binomial_distribution() : negative_binomial_distribution(1) {}  // (1) (since C++11)
explicit negative_binomial_distribution( IntType k, double p = 0.5 );  // (2) (since C++11)
explicit negative_binomial_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `k` and `p` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **k** — the *k* distribution parameter (number of trial successes)
- **p** — the *p* distribution parameter (probability of a trial generating
  `true`)
- **params** — the distribution parameter set

### Notes

Requires that 0 < p ≤ 1 and 0 < k.

If `p == 1`, subsequent calls to the `operator()` overload that does not accept
a `param_type` object will cause undefined behavior.

The default-constructed `std::negative_binomial_distribution` is equivalent to
the default-constructed `std::geometric_distribution`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/negative_binomial_distribution/negative_binomial_distribution*
