# iter_move(std::basic_const_iterator<Iter>)

```cpp
friend constexpr /* rvalue-reference */
    iter_move( const basic_const_iterator& i ) noexcept(/* see below */);  // (since C++23)
```

Casts the result of dereferencing the underlying iterator to its associated
const rvalue reference type.

The return type `/* rvalue-reference */` is
`std::common_reference_t<const std::iter_value_t<Iter>&&,
std::iter_rvalue_reference_t<Iter>>`.

The function body is equivalent to
`return static_cast</* rvalue-reference */>(std::ranges::iter_move(i.base()));`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::basic_const_iterator<Iter>` is an associated class of the arguments.

### Parameter

- **i** — a `basic_const_iterator`

### Return value

An rvalue reference to const, or a prvalue.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(static_cast</* rvalue-reference
*/>(std::ranges::iter_move(i.base()))))`

### Example

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/iter_move*
