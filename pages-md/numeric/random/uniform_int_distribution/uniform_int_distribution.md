# std::uniform_int_distribution<IntType>::uniform_int_distribution

```cpp
uniform_int_distribution() : uniform_int_distribution(0) { }  // (1) (since C++11)
explicit uniform_int_distribution( IntType a,
                                   IntType b = std::numeric_limits<IntType>::max() );  // (2) (since C++11)
explicit uniform_int_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs new distribution object.

2) Uses `a` and `b` as the distribution parameters.

3) Uses `params` as the distribution parameters.

The behavior is undefined if `a > b`.

### Parameters

- **a** — the *a* distribution parameter (minimum value)
- **b** — the *b* distribution parameter (maximum value)
- **params** — the distribution parameter set

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_int_distribution/uniform_int_distribution*
