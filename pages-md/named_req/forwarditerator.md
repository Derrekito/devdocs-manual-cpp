# C++ named requirements: LegacyForwardIterator

A **LegacyForwardIterator** is a LegacyIterator that can read data from the
pointed-to element.

Unlike LegacyInputIterator and LegacyOutputIterator, it can be used in multipass
algorithms.

If a **LegacyForwardIterator** `it` originates from a Container, then `it`'s
`value_type` is the same as the container's, so dereferencing (`*it`) obtains
the container's `value_type`.

### Requirements

The type `It` satisfies LegacyForwardIterator if

- The type `It` satisfies LegacyInputIterator
- The type `It` satisfies DefaultConstructible
- Objects of the type `It` provide *multipass guarantee* described below
- Let `T` be the value type of `It`. The type
  std::iterator_traits<It>::reference must be either

  - `T&` or `T&&`(since C++11) if `It` satisfies LegacyOutputIterator (`It` is
    mutable), or
  - const T& or `const T&&`(since C++11) otherwise (`It` is constant),

  (where `T` is the type denoted by std::iterator_traits<It>::value_type)

- Equality and inequality comparison is defined over all iterators for the same
  underlying sequence and the value initialized-iterators(since C++14).

And, given

- `i`, dereferenceable lvalue of type `It`
- `reference`, the type denoted by std::iterator_traits<It>::reference

The following expressions must be valid and have their specified effects

  Expression | Return type | Equivalent expression
  `i++` | `It` | `It ip = i; ++i; return ip;`
  `*i++` | `reference`

A *mutable* LegacyForwardIterator is a LegacyForwardIterator that additionally
satisfies the LegacyOutputIterator requirements.

### Multipass guarantee

Given `a` and `b`, dereferenceable iterators of type `It`:

- If `a` and `b` compare equal (`a == b` is contextually convertible to `true`)
  then either they are both non-dereferenceable or `*a` and `*b` are references
  bound to the same object.
- If `*a` and `*b` refer to the same object, then `a == b`.
- Assignment through a mutable `ForwardIterator` iterator cannot invalidate the
  iterator (implicit due to `reference` defined as a true reference).
- Incrementing a copy of `a` does not change the value read from `a` (formally,
  either `It` is a raw pointer type or the expression `(void)++It(a), *a` is
  equivalent to the expression `*a`).
- `a == b` implies `++a == ++b`.

### Singular iterators
A value-initialized LegacyForwardIterator behaves like the past-the-end iterator
of some unspecified empty container: it compares equal to all value-initialized
LegacyForwardIterators of the same type.
*(since C++14)*

### Concept
For the definition of `std::iterator_traits`, the following exposition-only
concept is defined.
```cpp
template<class It>
concept __LegacyForwardIterator =
    __LegacyInputIterator<It> && std::constructible_from<It> &&
    std::is_reference_v<std::iter_reference_t<It>> &&
    std::same_as<
        std::remove_cvref_t<std::iter_reference_t<It>>,
        typename std::indirectly_readable_traits<It>::value_type> &&
    requires(It it) {
        {  it++ } -> std::convertible_to<const It&>;
        { *it++ } -> std::same_as<std::iter_reference_t<It>>;
    };
```
where the exposition-only concept `__LegacyInputIterator<T>` is described in
LegacyInputIterator#Concept.
*(since C++20)*

### Notes

Unlike the `std::forward_iterator` concept, the **LegacyForwardIterator**
requirements requires dereference to return a reference.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1212 (N3066) | C++98 | the return type of `*i++` did not match the return
      type of `*i--` required by LegacyBidirectionalIterator | changed the
      return type to `reference`
  LWG 1311 (N3066) | C++98 | '`a == b` implies `++a == ++b`' alone did not offer
      multipass guarantee[1] | also requires '`a == b` implies `++a != b`'[2]
  LWG 3798 | C++20 | `__LegacyForwardIterator` required
      std::iter_reference_t<It> to be an lvalue reference type | also allows
      rvalue reference types

1. In the scenario where `a` and `b` use the same underlying iterator,
   evaluating the expression `++a == ++b` actually increments the underlying
   container twice, but the result is still `true`.
1. Formally also requires implying `++b != a`.

### See also

- **forward_iterator (C++20)** — specifies that an `input_iterator` is a forward
  iterator, supporting equality comparison and multi-pass (concept)
- ****Iterator library**** — provides definitions for iterators, iterator
  traits, adaptors, and utility functions

---
*Source: https://en.cppreference.com/w/cpp/named_req/ForwardIterator*
