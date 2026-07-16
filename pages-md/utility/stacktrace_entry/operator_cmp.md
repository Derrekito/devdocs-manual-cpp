# operator==, operator<=>(std::stacktrace_entry)

```cpp
friend constexpr bool operator==( const stacktrace_entry& lhs,
                                  const stacktrace_entry& rhs ) noexcept;  // (1) (since C++23)
friend constexpr std::strong_ordering
    operator<=>( const stacktrace_entry& lhs, const stacktrace_entry& rhs ) noexcept;  // (2) (since C++23)
```

1) Compares `lhs` and `rhs` for equality. Two `stacktrace_entry` values are
   equal if and only if they represent the same stacktrace entry, or both of
   them are empty.

2) Gets the relative order between `lhs` and `rhs` in the unspecified strict
   total order over all `stacktrace_entry` values which is consistent with the
   equality relation established by `operator==`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::stacktrace_entry` is
an associated class of the arguments.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Parameters

- **lhs, rhs** — `stacktrace_entry` values to compare

### Return value

1) `true` if two `lhs` and `rhs` compare equal, `false` otherwise.

2) `std::strong_ordering::equal` if `lhs` and `rhs` compare equal.

Otherwise, `std::strong_ordering::less` if `lhs` is ordered before `rhs` in the
   strict total order.

Otherwise, `std::strong_ordering::greater` (in which case `rhs` is ordered
   before `lhs` in the strict total order).

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/operator_cmp*
