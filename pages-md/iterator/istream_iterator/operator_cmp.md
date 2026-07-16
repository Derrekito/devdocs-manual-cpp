# operator==,!=(std::istream_iterator<T, CharT, Traits, Dist>)

```cpp
template< class T, class CharT, class Traits, class Dist >
bool operator==( const std::istream_iterator<T, CharT, Traits, Dist>& lhs,
                 const std::istream_iterator<T, CharT, Traits, Dist>& rhs );  // (1)
template< class T, class CharT, class Traits, class Dist >
bool operator!=( const std::istream_iterator<T, CharT, Traits, Dist>& lhs,
                 const std::istream_iterator<T, CharT, Traits, Dist>& rhs );  // (2) (until C++20)
friend bool operator==( const istream_iterator& i, std::default_sentinel_t );  // (3) (since C++20)
```

Checks whether both `lhs` and `rhs` are equal. Two stream iterators are equal if
both of them are end-of-stream iterators or both of them refer to the same
stream.

1) Checks whether `lhs` is *equal to* `rhs`.

2) Checks whether `lhs` is *not equal to* `rhs`.

3) Checks whether `lhs` is an end-of-stream iterator. This function is not
   visible to ordinary unqualified or qualified lookup, and can only be found by
   argument-dependent lookup when `std::istream_iterator<T, CharT, Traits,
   Dist>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — stream iterators to compare

### Return value

1) `true` if `lhs` is *equal to* `rhs`, `false` otherwise.

2) `true` if `lhs` is *not equal to* `rhs`, `false` otherwise.

3) `true` if `lhs` is an end-of-stream iterator, `false` otherwise.

### Exceptions

May throw implementation-defined exceptions.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 261 | C++98 | the description of `operator!=` was missing | added

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator/operator_cmp*
