# std::linear_congruential_engine<UIntType,a,c,m>::linear_congruential_engine

```cpp
linear_congruential_engine() : linear_congruential_engine(default_seed) {}  // (1) (since C++11)
explicit linear_congruential_engine( result_type value );  // (2) (since C++11)
template< class Sseq >
explicit linear_congruential_engine( Sseq& s );  // (3) (since C++11)
linear_congruential_engine( const linear_congruential_engine& );  // (4) (since C++11) (implicitly declared)
```

Constructs the pseudo-random number engine.

1) Default constructor. Seeds the engine with `default_seed`.

The overload (3) only participates in overload resolution if `Sseq` qualifies as
a SeedSequence. In particular, it is excluded from the set of candidate
functions if `Sseq` is convertible to `result_type`.

### Parameters

- **value** — seed value to use in the initialization of the internal state
- **s** — seed sequence to use in the initialization of the internal state

### Complexity

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

### See also

- **seed (C++11)** — sets the current state of the engine (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/linear_congruential_engine/linear_congruential_engine*
