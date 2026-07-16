# std::ranges::views::empty, std::ranges::empty_view

```cpp
template<class T>
    requires std::is_object_v<T>
class empty_view : public ranges::view_interface<empty_view<T>>  // (1) (since C++20)
namespace views {
    template<class T> inline constexpr empty_view<T> empty{};
}  // (2) (since C++20)
```

1) A range factory that produces a `view` of no elements of a particular type.

2) Variable template for `empty_view`.

### Member functions

- **begin [static] (C++20)** — returns `nullptr` (public static member function)
- **end [static] (C++20)** — returns `nullptr` (public static member function)
- **data [static] (C++20)** — returns `nullptr` (public static member function)
- **size [static] (C++20)** — returns `​0​` (zero) (public static member
  function)
- **empty [static] (C++20)** — returns `true` (public static member function)

**Inherited from `std::ranges::view_interface`**

- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

## std::ranges::empty_view::begin

```cpp
static constexpr T* begin() noexcept { return nullptr; }  // (since C++20)
```

`empty_view` does not reference any element.

## std::ranges::empty_view::end

```cpp
static constexpr T* end() noexcept { return nullptr; }  // (since C++20)
```

`empty_view` does not reference any element.

## std::ranges::empty_view::data

```cpp
static constexpr T* data() noexcept { return nullptr; }  // (since C++20)
```

`empty_view` does not reference any element.

## std::ranges::empty_view::size

```cpp
static constexpr std::size_t size() noexcept { return 0; }  // (since C++20)
```

`empty_view` is always empty.

## std::ranges::empty_view::empty

```cpp
static constexpr bool empty() noexcept { return true; }  // (since C++20)
```

`empty_view` is always empty.

### Helper templates

```cpp
template<class T>
inline constexpr bool enable_borrowed_range<ranges::empty_view<T>> = true;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `empty_view`
satisfy `borrowed_range`.

### Notes

Although `empty_view` obtains `front`, `back`, and `operator[]` member functions
from `view_interface`, calls to them always result in undefined behavior since
an `empty_view` is always empty.

The inherited `operator bool` conversion function always returns `false`.

### Example

```cpp
#include <ranges>

int main()
{
    std::ranges::empty_view<long> e;
    static_assert(std::ranges::empty(e));
    static_assert(0 == e.size());
    static_assert(nullptr == e.data());
    static_assert(nullptr == e.begin());
    static_assert(nullptr == e.end());
}
```

### See also

- **ranges::single_viewviews::single (C++20)** — a `view` that contains a single
  element of a specified value (class template) (customization point object)
- **views::all_tviews::all (C++20)** — a `view` that includes all elements of a
  `range` (alias template) (range adaptor object)
- **ranges::ref_view (C++20)** — a `view` of the elements of some other `range`
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/empty_view*
