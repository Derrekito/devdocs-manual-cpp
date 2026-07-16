# std::span

`std::span` (C++20) is a non-owning view over a contiguous sequence of
objects — a pointer-and-size pair (or just a pointer, if the size is
known at compile time) wrapped in a container-like interface. It never
owns the memory it points to: if the array, container, or object the
span refers to is destroyed, resized, or reallocated, the span is left
dangling. A `span` can have a *static* extent, where the element count
is part of the type, or a *dynamic* extent (`std::dynamic_extent`),
where the count is stored at runtime.

```cpp skip
std::span<int> s(arr);              // dynamic extent, deduced from arr
std::span<int, 5> s(arr);           // static extent, fixed at compile time
std::span<int> s(v.data(), v.size());

s.size(); s.size_bytes();
s[i];                                // unchecked access
s.first(n); s.last(n);               // subspan of the first/last n elements
s.subspan(offset, count);            // arbitrary subspan
```

### Guarantees and costs

- A dynamic-extent `span` typically stores a pointer and a size; a
  static-extent `span` may store only a pointer, since the size is
  encoded in the type.
- Every specialization of `span` is TriviallyCopyable (formally
  required since C++23; every existing implementation already
  satisfied this earlier).
- `span` satisfies `borrowed_range` and `view` from `<ranges>`.
- `element_type` is `T`; `iterator` is a LegacyRandomAccessIterator and
  `contiguous_iterator`, mutable when `T` is not const-qualified.

### Gotchas

- `span` never owns what it points to. Returning a `span` into a local
  container, or holding one past the lifetime of the container it was
  built from, produces a dangling view with no diagnostic — the same
  hazard as returning a raw pointer or reference.
- A `span` over a container that reallocates (e.g. a `vector` that
  grows via `push_back`) dangles just as an iterator would, even
  though nothing about the `span` itself changed.
- `operator[]` is unchecked; there is no bounds-checked `at()` before
  C++26.
- `T` must not be an abstract class type, and must be a complete
  object type.

### Example

```cpp c++20
#include <algorithm>
#include <cstddef>
#include <iostream>
#include <span>

template<class T, std::size_t N>
constexpr auto slide(std::span<T, N> s, std::size_t offset, std::size_t width)
{
    return s.subspan(offset, offset + width <= s.size() ? width : 0U);
}

void println(const auto& seq)
{
    bool first = true;
    for (const auto& elem : seq)
    {
        if (!first)
            std::cout << ' ';
        first = false;
        std::cout << elem;
    }
    std::cout << '\n';
}

int main()
{
    constexpr int a[]{0, 1, 2, 3, 4, 5, 6, 7, 8};
    constexpr std::size_t width{6};

    for (std::size_t offset{}; ; ++offset)
        if (auto s = slide(std::span{a}, offset, width); !s.empty())
            println(s);
        else
            break;
}
```

```text
0 1 2 3 4 5
1 2 3 4 5 6
2 3 4 5 6 7
3 4 5 6 7 8
```

### Reference

```cpp skip
template<
    class T,
    std::size_t Extent = std::dynamic_extent
> class span;  // (since C++20)
```

`T` must be a complete object type that is not an abstract class.
`Extent` is the element count, or `std::dynamic_extent` for a
runtime-determined size. `value_type` is `std::remove_cv_t<T>`;
`pointer`/`reference` are `T*`/`T&` (const-qualified for the const
member types). `extent` is a `static constexpr std::size_t` member
holding `Extent`.

**Member functions**, grouped as upstream groups them:

- (constructor), `operator=`, (destructor, implicitly declared)

  Iterators
  - `begin`/`cbegin` (C++23), `end`/`cend` (C++23)
  - `rbegin`/`crbegin` (C++23), `rend`/`crend` (C++23)

  Element access
  - `front`, `back`
  - `at` (C++26) — bounds-checked, unlike `operator[]`
  - `operator[]` — unchecked access
  - `data`

  Observers
  - `size`, `size_bytes`, `empty`

  Subviews
  - `first`, `last` — subspan of the first/last N elements
  - `subspan` — arbitrary subspan

Non-member: `as_bytes`/`as_writable_bytes` (C++20) view the span as
bytes. `dynamic_extent` is the constant marking dynamic extent.
`ranges::enable_borrowed_range` and `ranges::enable_view`
specializations make every `span` satisfy `borrowed_range` and `view`.

### See also

- **mdspan** (C++23) — a multi-dimensional non-owning array view
- **ranges::subrange** (C++20) — combines an iterator-sentinel pair
  into a `view`
- **initializer_list** (C++11) — view over a temporary array created
  by list-initialization
- **basic_string_view** (C++17) — read-only string view

---
*Source: https://en.cppreference.com/w/cpp/container/span*
