# std::exponential_distribution<RealType>::exponential_distribution

```cpp
exponential_distribution() : exponential_distribution(1.0) {}  // (1) (since C++11)
explicit exponential_distribution( RealType lambda );  // (2) (since C++11)
explicit exponential_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `lambda` as the distribution parameter.

3) Uses `params` as the distribution parameter.

### Parameters

- **lambda** — the *λ* distribution parameter (the rate parameter)
- **params** — the distribution parameter set

### Notes

Requires that 0 < lambda.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/exponential_distribution/exponential_distribution*
