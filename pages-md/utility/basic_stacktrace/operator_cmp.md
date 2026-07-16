# operator==, operator<=>(std::basic_stacktrace)

```cpp
template< class Allocator2 >
friend bool operator==( const basic_stacktrace& lhs,
                        const basic_stacktrace<Allocator2>& rhs ) noexcept;  // (1) (since C++23)
template< class Allocator2 >
friend std::strong_ordering
    operator<=>( const basic_stacktrace& lhs,
                 const basic_stacktrace<Allocator2>& rhs ) noexcept;  // (2) (since C++23)
```

1) Checks if the contents of `lhs` and `rhs` are equal, that is, they have the
   same number of elements and each element in `lhs` compares equal with the
   element in `rhs` at the same position.

Equivalent to `return std::equal(lhs.begin(), lhs.end(), rhs.begin(),
   rhs.end());`.

2) Returns the relative order of the numbers of stacktrace entries in `lhs` and
   `rhs` if they are not equal. Otherwise (if the numbers of elements of `lhs`
   and `rhs` are equal), returns the lexicographical order of the elements of
   `lhs` and `rhs`.

Equivalent to `if (auto cmp = lhs.size() <=> rhs.size(); cmp != 0) return cmp;
   else return std::lexicographical_compare_three_way(lhs.begin(), lhs.end(),
   rhs. begin(), rhs.end());`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::basic_stacktrace<Allocator>` is an associated class of the arguments.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Parameters

- **lhs, rhs** — `basic_stacktrace`s whose contents to compare

### Return value

1) `true` if the contents of `lhs` and `rhs` are equal, `false` otherwise.

2) `lhs.size() <=> rhs.size()` if the result is not `std::strong_order::equal`,
   the lexicographical order of the elements of `lhs` and `rhs` otherwise.

### Complexity

1,2) Constant if `lhs` and `rhs` are of different size, linear in the size of
   `lhs` otherwise.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/operator_cmp*
