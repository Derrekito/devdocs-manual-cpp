# std::bernoulli_distribution::bernoulli_distribution

```cpp
bernoulli_distribution() : bernoulli_distribution(0.5) { }  // (1) (since C++11)
explicit bernoulli_distribution( double p );  // (2) (since C++11)
explicit bernoulli_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs new distribution object.

2) Uses `p` as the distribution parameter.

3) Uses `params` as the distribution parameter.

### Parameters

- **p** — the *p* distribution parameter (probability of generating `true`)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/bernoulli_distribution/bernoulli_distribution*
