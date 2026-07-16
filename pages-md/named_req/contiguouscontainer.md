# C++ named requirements: ContiguousContainer (since C++17)

A **ContiguousContainer** is a Container that stores objects in contiguous
memory locations.

### Requirements

The type `X` satisfies ContiguousContainer if

- The type `X` satisfies Container
- The type `X` supports LegacyRandomAccessIterators
- The member types `X::iterator` and `X::const_iterator` are
  LegacyContiguousIterators(until C++20)`contiguous_iterator`s(since C++20)

### Contiguous containers in the standard library

- **basic_string** — stores and manipulates sequences of characters (class
  template)
- **array (C++11)** — static contiguous array (class template)
- **vector** — dynamic contiguous array (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/ContiguousContainer*
