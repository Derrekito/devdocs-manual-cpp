# Standard library header <utility>

`<utility>` is the general utility library, and it shows: rather than
one coherent facility, it's a grab-bag of small, unrelated pieces that
don't warrant their own header — `std::move`/`std::forward` (the
building blocks of move semantics), `std::pair`, `std::swap`, the
in-place/piecewise construction tag types used by `optional`/`variant`/
`any`/`pair`, and a handful of safe-integer-comparison helpers. You'll
pull it in indirectly all the time (most headers that need `pair` or
`move`/`forward` include it for you), but include it directly whenever
you write generic code that needs `std::forward` or `std::move`.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support
- **`<initializer_list>`** (C++11) — `std::initializer_list` class
  template

### Namespaces

- **`rel_ops`** — deprecated automatic comparison-operator generation
- **operator!=** / **operator>** / **operator<=** / **operator>=**
  (deprecated in C++20) — synthesizes the remaining comparisons from a
  user-defined `operator==` and `operator<`; superseded by `<=>`

### Move and forwarding

- **move** (C++11) — obtains an rvalue reference to its argument
- **forward** (C++11) — forwards a function argument, preserving its
  value category
- **forward_like** (C++23) — forwards an argument as if cast to the
  value category and constness of another specified type
- **move_if_noexcept** (C++11) — an rvalue reference only if the move
  constructor is `noexcept`, otherwise a `const` lvalue reference
- **as_const** (C++17) — a `const` reference to its argument
- **declval** (C++11) — a reference to its argument, for unevaluated
  contexts only (`decltype`, `sizeof`, ...)

### Swap and exchange

- **swap** — swaps the values of two objects
- **exchange** (C++14) — replaces an object's value, returning the
  previous value

### Integer utilities

- **cmp_equal** / **cmp_not_equal** / **cmp_less** / **cmp_greater** /
  **cmp_less_equal** / **cmp_greater_equal** (C++20) — compares two
  integers of possibly different signedness without the value change a
  plain conversion could cause
- **in_range** (C++20) — is an integer value representable in a given
  integer type?
- **to_underlying** (C++23) — converts an enum to its underlying type
- **unreachable** (C++23) — marks a point of execution as unreachable

### Class template `std::pair`

- **pair** — a 2-element heterogeneous tuple
- **make_pair** — creates a `pair`, deducing its element types
- **tuple_size** / **tuple_element** (C++11) — generic tuple-like size
  and element-type access, specialized here for `pair`
- **get(pair)** (C++11) — accesses a `pair` element by index or type
- **operator==** and the relational operators — lexicographically
  compares two `pair`s (`operator<=>` added in C++20; the other
  relational overloads were removed then, synthesized from it)
- **std::swap(pair)** (C++11) — specializes `std::swap`

### Compile-time integer sequences

- **integer_sequence** (C++14) — a compile-time sequence of integers,
  with `index_sequence`, `make_integer_sequence`, `make_index_sequence`,
  and `index_sequence_for` helper aliases built on top of it

### Construction tags

- **piecewise_construct** / **piecewise_construct_t** (C++11) — tells a
  `pair` constructor to build each member in place from a `tuple` of
  arguments
- **in_place** / **in_place_type** / **in_place_index** / **in_place_t**
  / **in_place_type_t** / **in_place_index_t** (C++17) — tells
  `optional`/`variant`/`any` to construct their contained object in
  place
- **nontype** / **nontype_t** (C++26) — tag for passing a non-type
  template parameter as a value

### Forward declarations

- **tuple** (C++11) — fixed-size heterogeneous container, defined in
  `<tuple>`

### See also

- **`<tuple>`** (C++11) — `std::tuple` class template

---
*Source: https://en.cppreference.com/w/cpp/header/utility*
