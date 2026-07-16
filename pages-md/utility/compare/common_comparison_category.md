# std::common_comparison_category

```cpp
template< class... Ts >
struct common_comparison_category {
    using type = /* see below */ ;
};  // (since C++20)
```

The class template `std::common_comparison_category` provides an alias (as the
member typedef `type`) for the strongest comparison category to which all of the
template arguments `Ts...` can be converted.

In detail, the common comparison type of a list of n types `T`0...`T`n-1 is
defined as follows:

- If any `T`i is not a comparison category type (`std::partial_ordering`,
  `std::weak_ordering`, `std::strong_ordering`), `U` is `void`.
- Otherwise, if at least one `T`i is `std::partial_ordering`, `U` is
  `std::partial_ordering`.
- Otherwise, if at least one `T`i is `std::weak_ordering`, `U` is
  `std::weak_ordering`.
- Otherwise (if every `T`i is `std::strong_ordering`, or if the list is empty),
  `U` is `std::strong_ordering`.

### Template parameters

- **...Ts** — a possibly empty list of types

### Helper template

```cpp
template< class... Ts >
using common_comparison_category_t = typename common_comparison_category<Ts...>::type;  // (since C++20)
```

### Member types

- **`type`** — the strongest common comparison category (as defined above)

### Possible implementation

```cpp
namespace detail
{
    template<unsigned int>
    struct common_cmpcat_base     { using type = void; };
    template<>
    struct common_cmpcat_base<0u> { using type = std::strong_ordering; };
    template<>
    struct common_cmpcat_base<2u> { using type = std::partial_ordering; };
    template<>
    struct common_cmpcat_base<4u> { using type = std::weak_ordering; };
    template<>
    struct common_cmpcat_base<6u> { using type = std::partial_ordering; };
} // namespace detail

template<class...Ts>
struct common_comparison_category :
    detail::common_cmpcat_base<(0u | ... |
        (std::is_same_v<Ts, std::strong_ordering>  ? 0u :
         std::is_same_v<Ts, std::weak_ordering>    ? 4u :
         std::is_same_v<Ts, std::partial_ordering> ? 2u : 1u)
    )> {};
```

### Example

### See also

- **strong_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators and is substitutable (class)
- **weak_ordering (C++20)** — the result type of 3-way comparison that supports
  all 6 operators and is not substitutable (class)
- **partial_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators, is not substitutable, and allows incomparable values
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/common_comparison_category*
