# C++ named requirements: LegacyIterator

The **LegacyIterator** requirements describe types that can be used to identify
and traverse the elements of a container.

LegacyIterator is the base set of requirements used by other iterator types:
LegacyInputIterator, LegacyOutputIterator, LegacyForwardIterator,
LegacyBidirectionalIterator, and LegacyRandomAccessIterator. Iterators can be
thought of as an abstraction of pointers.

All the categories of iterators require only those functions that are realizable
for a given category in constant time (amortized). Therefore, requirement tables
and concept definitions (since C++20)for the iterators do not specify
complexity.

### Requirements

The type `It` satisfies LegacyIterator if

- The type `It` satisfies CopyConstructible, and
- The type `It` satisfies CopyAssignable, and
- The type `It` satisfies Destructible, and
- lvalues of type `It` satisfy Swappable, and
- `std::iterator_traits<It>` has member typedefs `value_type`(until C++20),
  `difference_type`, `reference`, `pointer`, and `iterator_category`, and
- Given `r`, an lvalue of type `It`, the following expressions must be valid and
  have their specified effects:

  Expression | Return Type | Precondition
  `*r` | unspecified | `r` is dereferenceable
  `++r` | `It&` | `r` is *incrementable* (the behavior of the expression `++r`
      is defined)

### Concept
For the definition of `std::iterator_traits`, the following exposition-only
concept is defined.
```cpp
template<class I>
concept __LegacyIterator =
    requires(I i)
    {
        {   *i } -> __Referenceable;
        {  ++i } -> std::same_as<I&>;
        { *i++ } -> __Referenceable;
    } && std::copyable<I>;
```
where the exposition-only concept `__Referenceable<T>` is satisfied if and only
if `T&` is a valid type (in particular, `T` must not be `void`).
*(since C++20)*

### Defect Reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3420 | C++20 | the exposition-only concept checks `copyable` first |
      `copyable` is checked only if the requires-expression yields true

### See also

- **input_or_output_iterator (C++20)** — specifies that objects of a type can be
  incremented and dereferenced (concept)
- ****Iterator library**** — provides definitions for iterators, iterator
  traits, adaptors, and utility functions

---
*Source: https://en.cppreference.com/w/cpp/named_req/Iterator*
