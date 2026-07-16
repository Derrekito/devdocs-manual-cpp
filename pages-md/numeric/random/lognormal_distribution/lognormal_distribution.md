# std::lognormal_distribution<RealType>::lognormal_distribution

```cpp
lognormal_distribution() : lognormal_distribution(0.0) {}  // (1) (since C++11)
explicit lognormal_distribution( RealType m, RealType s = 1.0 );  // (2) (since C++11)
explicit lognormal_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `m` and `s` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **m** — the *m* distribution parameter (log-scale)
- **s** — the *s* distribution parameter (shape)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/lognormal_distribution/lognormal_distribution*
