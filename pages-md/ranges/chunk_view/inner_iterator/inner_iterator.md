# std::ranges::chunk_view<V>::*inner-iterator*::*inner-iterator*

```cpp
/*inner-iterator*/( /*inner-iterator*/&& other ) = default;  // (1) (since C++23)
private:
constexpr explicit /*inner-iterator*/( chunk_view& parent );  // (2) (exposition only*)
```

Construct an iterator.

1) A move constructor which move-initializes the underlying pointer `parent_`
   with `other.parent_`.

2) A private constructor which is used by `chunk_view::begin`. This constructor
   is not accessible to users. Initializes `parent_` with
   `std::addressof(parent)`.

### Parameters

- **other** — an iterator
- **parent** — the enclosing `ranges::chunk_view` object

### Example

### See also

- **operator= (C++23)** — move assigns another iterator (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/inner_iterator*
