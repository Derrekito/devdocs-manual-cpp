# std::binomial_distribution<IntType>::binomial_distribution

```cpp
binomial_distribution() : binomial_distribution(1) {}  // (1) (since C++11)
explicit binomial_distribution( IntType t, double p = 0.5 );  // (2) (since C++11)
explicit binomial_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `t` and `p` as the distribution parameters.

3) Uses `params` as the distribution parameter.

### Parameters

- **t** — the *t* distribution parameter (number of trials)
- **p** — the *p* distribution parameter (probability of a trial generating
  `true`)
- **params** — the distribution parameter set

### Notes

Requires that 0 ≤ p ≤ 1 and 0 ≤ t.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/binomial_distribution/binomial_distribution*
