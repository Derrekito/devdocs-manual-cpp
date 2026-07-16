# std::convertible_to

```cpp
template< class From, class To >
concept convertible_to =
    std::is_convertible_v<From, To> &&
    requires {
        static_cast<To>(std::declval<From>());
    };  // (since C++20)
```

The concept `convertible_to<From, To>` specifies that an expression of the same
type and value category as those of `std::declval<From>()` can be implicitly and
explicitly converted to the type `To`, and the two forms of conversion produce
equal results.

### Semantic requirements

`convertible_to<From, To>` is modeled only if, given a function `fun` of type
`std::add_rvalue_reference_t<From>()` such that the expression `fun()` is
equality-preserving,

- Either `To` is neither an object type nor a reference-to-object type, or
  `static_cast<To>(fun())` is equal to `[]() -> To { return fun(); }()`, and
- One of the following is true: `std::add_rvalue_reference_t<From>` is not a
  reference-to-object type, or `std::add_rvalue_reference_t<From>` is an rvalue
  reference to a non-const-qualified type, and the resulting state of the object
  referenced by `fun()` is valid but unspecified after either expression above;
  or the object referred to by `fun()` is not modified by either expression
  above.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### See also

- **is_convertibleis_nothrow_convertible (C++11)(C++20)** — checks if a type can
  be converted to the other type (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/convertible_to*
