# C++ named requirements: LegacyContiguousIterator (since C++17)

A **LegacyContiguousIterator** is a LegacyIterator whose logically adjacent
elements are also physically adjacent in memory.

A pointer to an element of an array satisfies all requirements of
LegacyContiguousIterator.

### Requirements

The type `It` satisfies LegacyContiguousIterator if

- The type `It` satisfies LegacyIterator

And, for every

- `a`, a dereferenceable iterator of type `It`
- `n`, an integral value

such that

- `a + n` is a valid and dereferenceable iterator value

then

- `*(a + n)` is equivalent to `*(std::addressof(*a) + n)`.

### Standard library

The following standard library types are **LegacyContiguousIterators**.

- `array::iterator`.
- `basic_string_view::iterator`.
- `basic_string::iterator`.
- `vector::iterator` for `value_type` other than `bool`.
- The return type of `begin(valarray)` and `end(valarray)`.

### Notes

Fancy pointer to object types are also required to satisfy
LegacyContiguousIterator.

LegacyContiguousIterator is replaced with the `contiguous_iterator` concept:
iterator types in the standard library that are required to satisfy
LegacyContiguousIterator in C++17 are required to model `contiguous_iterator` in
C++20.
*(since C++20)*

### See also

- **contiguous_iterator (C++20)** — specifies that a `random_access_iterator` is
  a contiguous iterator, referring to elements that are contiguous in memory
  (concept)
- ****Iterator library**** — provides definitions for iterators, iterator
  traits, adaptors, and utility functions

---
*Source: https://en.cppreference.com/w/cpp/named_req/ContiguousIterator*
