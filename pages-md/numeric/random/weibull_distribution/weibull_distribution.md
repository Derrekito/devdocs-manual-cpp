# std::weibull_distribution<RealType>::weibull_distribution

```cpp
weibull_distribution() : weibull_distribution(1.0) {}  // (1) (since C++11)
explicit weibull_distribution( RealType a, RealType b = 1.0 );  // (2) (since C++11)
explicit weibull_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `a` and `b` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **a** — the *a* distribution parameter (shape)
- **b** — the *b* distribution parameter (scale)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/weibull_distribution/weibull_distribution*
