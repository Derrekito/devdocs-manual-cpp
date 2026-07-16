# std::poisson_distribution<IntType>::poisson_distribution

```cpp
poisson_distribution() : poisson_distribution(1.0) {}  // (1) (since C++11)
explicit poisson_distribution( double mean );  // (2) (since C++11)
explicit poisson_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

1) Uses `1.0` as the distribution parameter.

2) Uses `mean` as the distribution parameter.

3) Uses `params` as the distribution parameter.

### Parameters

- **mean** — the *μ* distribution parameter (the mean of the distribution)
- **params** — the distribution parameter set

### Notes

Requires that `0 < mean`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/poisson_distribution/poisson_distribution*
