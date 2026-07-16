# C++ named requirements: LegacyBidirectionalIterator

A **LegacyBidirectionalIterator** is a LegacyForwardIterator that can be moved
in both directions (i.e. incremented and decremented).

If a **LegacyBidirectionalIterator** `it` originates from a Container, then
`it`'s `value_type` is the same as the container's, so dereferencing (`*it`)
obtains the container's `value_type`.

### Requirements

The type `It` satisfies LegacyBidirectionalIterator if

- The type `It` satisfies LegacyForwardIterator

And, given

- `a` and `b`, lvalues of type `It`
- `reference`, the type denoted by std::iterator_traits<It>::reference

The following expressions must be valid and have their specified effects:

  Expression | Return | Equivalent expression | Notes
  `--a` | `It&` | Preconditions: `a` is decrementable (there exists such `b`
      that `a == ++b`) Postconditions: `a` is dereferenceable `--(++a) == a` If
      `--a == --b` then `a == b` `a` and `--a` designate the same iterator
      object
  `a--` | convertible to const It& | `It temp = a; --a; return temp;`
  `*a--` | `reference`

A *mutable* LegacyBidirectionalIterator is a LegacyBidirectionalIterator that
additionally satisfies the LegacyOutputIterator requirements.

### Notes

The begin iterator is not decrementable and the behavior is undefined if
`--container.begin()` is evaluated.

A bidirectional iterator does not have to be dereferenceable to be decrementable
(in particular, the end iterator is not dereferenceable but is decrementable).

### Concept
For the definition of `std::iterator_traits`, the following exposition-only
concept is defined.
```cpp
template<class I>
concept __LegacyBidirectionalIterator =
    __LegacyForwardIterator<I> && requires(I i)
    {
        {  --i } -> std::same_as<I&>;
        {  i-- } -> std::convertible_to<const I&>;
        { *i-- } -> std::same_as<std::iter_reference_t<I>>;
    };
```
where the exposition-only concept `__LegacyForwardIterator` is described in
LegacyForwardIterator#Concept.
*(since C++20)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 299 (N3066) | C++98 | the return type of `*a--` was required to be
      convertible to `T` | changed the return type to `reference`[1]
  LWG 383 | C++98 | `b` was required to be dereferenceable after `--a` | `a` is
      required to be dereferenceable instead
  LWG 1212 (N3066) | C++98 | the return type of `*a--` did not match the return
      type of `*a++` required by LegacyForwardIterator | changed the return type
      to `reference`

1. This issue was initially resolved by N2758 (iterator concepts), which was
   dropped later from the C++ standard.

### See also

- **bidirectional_iterator (C++20)** — specifies that a `forward_iterator` is a
  bidirectional iterator, supporting movement backwards (concept)
- ****Iterator library**** — provides definitions for iterators, iterator
  traits, adaptors, and utility functions

---
*Source: https://en.cppreference.com/w/cpp/named_req/BidirectionalIterator*
