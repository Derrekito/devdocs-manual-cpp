# operator==,!=(std::discard_block_engine)

```cpp
friend bool operator==( const discard_block_engine& lhs,
                        const discard_block_engine& rhs );  // (1) (since C++11)
friend bool operator!=( const discard_block_engine& lhs,
                        const discard_block_engine& rhs );  // (2) (since C++11) (until C++20)
```

Compares two pseudo-random number engine adaptors. Two engine adaptors are
equal, if their underlying engines are equal and their internal state (if any)
is equal, that is, if they would generate equivalent values for any number of
calls of `operator()`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::discard_block_engine<Engine,p,r>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — engine adaptors to compare

### Return value

1) `true` if the engine adaptors are equivalent, `false` otherwise.

2) `true` if the engine adaptors are not equivalent, `false` otherwise.

### Exceptions

Throws nothing.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3519 | C++11 | the form of equality operators were unspecified | specified
      to be hidden friends

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/discard_block_engine/operator_cmp*
