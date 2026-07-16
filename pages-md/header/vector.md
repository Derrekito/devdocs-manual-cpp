# Standard library header <vector>

`<vector>` is part of the containers library and defines
`std::vector`, the dynamic contiguous array that's the default
sequence container in most C++ code — reach for it whenever you need a
resizable, randomly-indexable collection and don't have a specific
reason to want something else. Because storage is contiguous, growth
(`push_back` past capacity, `insert`, `resize`, ...) can reallocate and
invalidate existing iterators, pointers, and references; check
`capacity()` when that matters.

`vector<bool>` is a partial specialization that packs elements into
individual bits instead of storing `bool`s contiguously. It saves
space but its `operator[]` returns a proxy reference, not `bool&`, which
breaks generic code expecting real references — many style guides ban
it for exactly this reason.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support
- **`<initializer_list>`** (C++11) — `std::initializer_list` class
  template

### Classes

- **vector** — dynamic contiguous array
- **vector<bool>** — space-efficient bitset specialization, with a
  proxy `reference` type instead of `bool&`
- **std::hash<vector<bool>>** (C++11) — hash support for `vector<bool>`

### Functions

- **operator==** and the relational operators — lexicographically
  compares two `vector`s (`operator<=>` added in C++20; the other
  relational overloads were removed then, synthesized from it)
- **std::swap(vector)** — specializes `std::swap`
- **erase(vector)** / **erase_if(vector)** (C++20) — erases every
  element matching a value or predicate

### Range access

- **begin** / **cbegin** (C++11 / C++14) — iterator to the start of a
  container or array
- **end** / **cend** (C++11 / C++14) — iterator to the end
- **rbegin** / **crbegin** (C++14) — reverse iterator to the start
- **rend** / **crend** (C++14) — reverse iterator to the end
- **size** / **ssize** (C++17 / C++20) — element count (`ssize` returns
  a signed type)
- **empty** (C++17) — is the container empty?
- **data** (C++17) — pointer to the underlying array

---
*Source: https://en.cppreference.com/w/cpp/header/vector*
