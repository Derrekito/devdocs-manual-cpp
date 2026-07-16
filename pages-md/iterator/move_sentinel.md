# std::move_sentinel

```cpp
template< std::semiregular S >
class move_sentinel;  // (since C++20)
```

`std::move_sentinel` is a sentinel adaptor used for denoting ranges together
with `std::move_iterator`.

### Template parameters

- **S** — the type of underlying sentinel

### Member functions

- **(constructor) (C++20)** — constructs a new `move_sentinel` (public member
  function)
- **operator= (C++20)** — assigns the contents of one `move_sentinel` to another
  (public member function)
- **base (C++20)** — return a copy of the underlying sentinel (public member
  function)

### Member objects

- **`last` (private member object)** — underlying sentinel, the name is for
  exposition only

### Non-member functions

Notes: These functions are hidden friends of `std::move_iterator` and invisible
to ordinary unqualified or qualified lookup.

- **operator==(std::move_sentinel) (C++20)** — compares the underlying iterator
  and the underlying sentinel (function template)
- **operator-(std::move_sentinel) (C++20)** — computes the distance between the
  underlying iterator and the underlying sentinel (function template)

### Example

### See also

- **move_iterator (C++11)** — iterator adaptor which dereferences to an rvalue
  reference (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_sentinel*
