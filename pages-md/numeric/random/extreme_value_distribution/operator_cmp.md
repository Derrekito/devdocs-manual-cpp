# operator==,!=(std::extreme_value_distribution)

```cpp
friend bool operator==( const extreme_value_distribution& lhs,
                        const extreme_value_distribution& rhs );  // (1) (since C++11)
friend bool operator!=( const extreme_value_distribution& lhs,
                        const extreme_value_distribution& rhs );  // (2) (since C++11) (until C++20)
```

Compares two distribution objects. Two distribution objects are equal when
parameter values and internal state is the same.

1) Compares two distribution objects for equality.

2) Compares two distribution objects for inequality.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::extreme_value_distribution<ResultType>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — distribution objects to compare

### Return value

1) `true` if the distribution objects are equal, `false` otherwise.

2) `true` if the distribution objects are not equal, `false` otherwise.

### Complexity

Constant.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3519 | C++11 | the form of equality operators were unspecified (could be
      hidden friends or free function templates) | specified to be hidden
      friends

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/extreme_value_distribution/operator_cmp*
