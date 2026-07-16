# operator==,!=(std::istreambuf_iterator<CharT,Traits>)

```cpp
template< class CharT, class Traits >
bool operator==( const std::istreambuf_iterator<CharT,Traits>& lhs,
                 const std::istreambuf_iterator<CharT,Traits>& rhs );  // (1)
template< class CharT, class Traits >
bool operator!=( const std::istreambuf_iterator<CharT,Traits>& lhs,
                 const std::istreambuf_iterator<CharT,Traits>& rhs );  // (2) (until C++20)
friend bool operator==( const istreambuf_iterator& lhs,
                        std::default_sentinel_t );  // (3) (since C++20)
```

Checks whether both `lhs` and `rhs` are valid, or both are invalid, regardless
of the stream buffer objects they use.

1) Equivalent to `lhs.equal(rhs)`.

2) Equivalent to `!lhs.equal(rhs)`.

3) Checks whether `lhs` is invalid. Equivalent to
   `lhs.equal(istreambuf_iterator{})`. This function is not visible to ordinary
   unqualified or qualified lookup, and can only be found by argument-dependent
   lookup when `std::istreambuf_iterator<CharT,Traits>` is an associated class
   of the arguments.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — stream buffer iterators to compare

### Return value

1) The result of `lhs.equal(rhs)`.

2) The result of `!lhs.equal(rhs)`.

3) The result of `lhs.equal(istreambuf_iterator{})`.

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/iterator/istreambuf_iterator/operator_cmp*
