# std::normal_distribution<RealType>::normal_distribution

```cpp
normal_distribution() : normal_distribution(0.0) {}  // (1) (since C++11)
explicit normal_distribution( RealType mean, RealType stddev = 1.0 );  // (2) (since C++11)
explicit normal_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `mean` and `stddev` as the distribution parameters.

3) Uses `params` as the distribution parameters.

The behavior is undefined if `stddev` is not greater than zero.

### Parameters

- **mean** — the *μ* distribution parameter (mean)
- **stddev** — the *σ* distribution parameter (standard deviation)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/normal_distribution/normal_distribution*
