# std::gamma_distribution<RealType>::gamma_distribution

```cpp
gamma_distribution() : gamma_distribution(1.0) {}  // (1) (since C++11)
explicit gamma_distribution( RealType alpha, RealType beta = 1.0 );  // (2) (since C++11)
explicit gamma_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `alpha` and `beta` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **alpha** — the *α* distribution parameter (shape)
- **beta** — the *β* distribution parameter (scale)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/gamma_distribution/gamma_distribution*
