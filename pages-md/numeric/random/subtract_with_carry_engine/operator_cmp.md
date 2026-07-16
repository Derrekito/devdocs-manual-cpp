# operator==,!=(std::subtract_with_carry_engine)

```cpp
friend bool operator==( const subtract_with_carry_engine& lhs,
                        const subtract_with_carry_engine& rhs );  // (1) (since C++11)
friend bool operator!=( const subtract_with_carry_engine& lhs,
                        const subtract_with_carry_engine& rhs );  // (2) (since C++11) (until C++20)
```

Compares two pseudo-random number engines. Two engines are equal, if their
internal states are equivalent, that is, if they would generate equivalent
values for any number of calls of `operator()`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::subtract_with_carry_engine<UIntType,w,s,r>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — engines to compare

### Return value

1) `true` if the engines are equivalent, `false` otherwise.

2) `true` if the engines are not equivalent, `false` otherwise.

### Exceptions

Throws nothing.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3519 | C++11 | the form of equality operators were unspecified | specified
      to be hidden friends

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/subtract_with_carry_engine/operator_cmp*
