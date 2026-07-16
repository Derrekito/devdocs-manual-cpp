# std::ranges::common_range

```cpp
template< class T >
  concept common_range =
    ranges::range<T> && std::same_as<ranges::iterator_t<T>, ranges::sentinel_t<T>>;  // (since C++20)
```

The `common_range` concept is a refinement of `range` for which
`std::ranges::begin()` and `std::ranges::end()` return the same type (e.g. all
standard library containers).

### Example

```cpp
#include <ranges>

struct A {
    char* begin();
    char* end();
};
static_assert( std::ranges::common_range<A> );

struct B {
    char* begin();
    bool end();
};  // not a common_range: begin/end have different types
static_assert( not std::ranges::common_range<B> );

struct C {
    char* begin();
};  // not a common_range, not even a range: has no end()
static_assert( not std::ranges::common_range<C> );

int main() { }
```

### See also

- **ranges::common_viewviews::common (C++20)** — converts a `view` into a
  **`common_range`** (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_range*
