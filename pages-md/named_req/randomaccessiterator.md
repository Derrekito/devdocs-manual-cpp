# C++ named requirements: LegacyRandomAccessIterator

A **LegacyRandomAccessIterator** is a LegacyBidirectionalIterator that can be
moved to point to any element in constant time.

If a **LegacyRandomAccessIterator** `it` originates from a Container, then
`it`'s `value_type` is the same as the container's, so dereferencing (`*it`)
obtains the container's `value_type`.

A pointer to an element of an array satisfies all requirements of
LegacyRandomAccessIterator.

### Requirements

The type `It` satisfies LegacyRandomAccessIterator if

- The type `It` satisfies LegacyBidirectionalIterator

And, given

- `value_type`, the type denoted by std::iterator_traits<It>::value_type
- `difference_type`, the type denoted by
  std::iterator_traits<It>::difference_type
- `reference`, the type denoted by std::iterator_traits<It>::reference
- `i`, `a`, `b`, objects of type `It` or const It
- `r`, an lvalue of type `It`
- `n`, an integer of type `difference_type`

The following expressions must be valid and have their specified effects:

  Expression | Return type | Operational semantics | Notes
  `r += n` | `It&` | `difference_type m = n; if (m >= 0) while (m--) ++r; else
      while (m++) --r; return r;` | `n` can be both positive or negative The
      complexity is constant (that is, the implementation cannot actually
      execute the while loop shown in operational semantics)
  `a + n` `n + a` | `It` | `It temp = a; return temp += n;` | `n` can be both
      positive or negative `a + n == n + a`
  `r -= n` | `It&` | `return r += -n;` | The absolute value of `n` must be
      within the range of representable values of `difference_type`.
  `i - n` | `It` | `It temp = i; return temp -= n;`
  `b - a` | `difference_type` | `return n;` (see the precondition) |
      Precondition: there exists a value `n` of type `difference_type` such that
      `a + n == b` Postcondition: `b == a + (b - a)`.
  `i[n]` | convertible to `reference` | `*(i + n)`
  `a < b` | contextually convertible to bool | Equivalent to `return b - a > 0;`
      | Precondition: same as of `b - a` Strict total ordering relation: `!(a <
      a)` if `a < b` then `!(b < a)` if `a < b` and `b < c` then `a < c` `a < b`
      or `b < a` or `a == b` (exactly one of the expressions is true)
  `a > b` | contextually convertible to bool | `b < a` | Total ordering relation
      opposite to `a < b`
  `a >= b` | contextually convertible to bool | `!(a < b)`
  `a <= b` | contextually convertible to bool | `!(a > b)`

The above rules imply that LegacyRandomAccessIterator also implements
LessThanComparable.

A *mutable* LegacyRandomAccessIterator is a LegacyRandomAccessIterator that
additionally satisfies the LegacyOutputIterator requirements.

### Concept
For the definition of `std::iterator_traits`, the following exposition-only
concept is defined.
```cpp
template<class I>
concept __LegacyRandomAccessIterator =
    __LegacyBidirectionalIterator<I> && std::totally_ordered<I> &&
        requires(I i, typename std::incrementable_traits<I>::difference_type n)
        {
            { i += n } -> std::same_as<I&>;
            { i -= n } -> std::same_as<I&>;
            { i +  n } -> std::same_as<I>;
            { n +  i } -> std::same_as<I>;
            { i -  n } -> std::same_as<I>;
            { i -  i } -> std::same_as<decltype(n)>;
            {  i[n]  } -> std::convertible_to<std::iter_reference_t<I>>;
        };
```
where the exposition-only concept `__LegacyBidirectionalIterator` is described
in LegacyBidirectionalIterator#Concept.
*(since C++20)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 299 (N3066) | C++98 | the return type of `a[n]` was required to be
      convertible to const value_type& | the return type is required to be
      convertible to `reference`
  LWG 448 | C++98 | the return type of `a[n]` was required to be convertible to
      `value_type` | the return type is required to be convertible to const
      value_type&[1]
  LWG 1079 | C++98 | `b - a` was defined using `a < b`, resulted in circular
      definition | removed `a < b` from the definition

1. LWG issue 299 was reopened after this resolution.

### See also

- **random_access_iterator (C++20)** — specifies that a `bidirectional_iterator`
  is a random-access iterator, supporting advancement in constant time and
  subscripting (concept)
- ****Iterator library**** — provides definitions for iterators, iterator
  traits, adaptors, and utility functions

---
*Source: https://en.cppreference.com/w/cpp/named_req/RandomAccessIterator*
