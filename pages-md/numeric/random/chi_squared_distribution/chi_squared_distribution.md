# std::chi_squared_distribution<RealType>::chi_squared_distribution

```cpp
chi_squared_distribution() : chi_squared_distribution(1.0) {}  // (1) (since C++11)
explicit chi_squared_distribution( RealType n );  // (2) (since C++11)
explicit chi_squared_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `n` as the distribution parameter.

3) Uses `params` as the distribution parameter.

### Parameters

- **n** — the *n* distribution parameter (degrees of freedom)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/chi_squared_distribution/chi_squared_distribution*
