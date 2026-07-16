# std::fisher_f_distribution<RealType>::fisher_f_distribution

```cpp
fisher_f_distribution() : fisher_f_distribution(1.0) {}  // (1) (since C++11)
explicit fisher_f_distribution( RealType m, RealType n = 1.0 );  // (2) (since C++11)
explicit fisher_f_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `m` and `n` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **m** — the *m* distribution parameter (degrees of freedom)
- **n** — the *n* distribution parameter (degrees of freedom)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/fisher_f_distribution/fisher_f_distribution*
