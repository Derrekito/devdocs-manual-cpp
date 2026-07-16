# std::uniform_real_distribution<RealType>::uniform_real_distribution

```cpp
uniform_real_distribution() : uniform_real_distribution(0.0) { }  // (1) (since C++11)
explicit uniform_real_distribution( RealType a, RealType b = 1.0 );  // (2) (since C++11)
explicit uniform_real_distribution( const param_type& params );  // (3) (since C++11)
```

Constructs a new distribution object.

2) Uses `a` and `b` as the distribution parameters.

3) Uses `params` as the distribution parameters.

### Parameters

- **a** — the *a* distribution parameter (minimum value)
- **b** — the *b* distribution parameter (maximum value)
- **params** — the distribution parameter set

### Notes

Requires that `a ≤ b` and `b - a ≤ std::numeric_limits<RealType>::max()`.

If `a == b`, subsequent calls to the `operator()` overload that does not accept
a `param_type` object will cause undefined behavior.

To create a distribution over the closed interval [a,b], `std::nextafter(b,
std::numeric_limits<RealType>::max())` may be used as the second parameter.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_real_distribution/uniform_real_distribution*
