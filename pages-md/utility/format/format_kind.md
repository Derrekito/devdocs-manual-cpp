# std::format_kind

```cpp
template< class R >
inline constexpr /* unspecified */ format_kind = /* unspecified */;  // (1) (since C++23)
template< ranges::input_range R >
    requires std::same_as<R, std::remove_cvref_t<R>>
inline constexpr range_format format_kind<R> = /* see description */;  // (2) (since C++23)
```

The variable template `format_kind` selects an appropriate `std::range_format`
for a range `R`.

`format_kind<R>` is defined as follows:

- If `std::same_as<std::remove_cvref_t<ranges::range_reference_t<R>>, R>` is
  true, `format_kind<R>` is `range_format::disabled`.
- Otherwise, if `R::key_type` is valid and denotes a type: If `R::mapped_type`
  is valid and denotes a type, let `U` be
  `std::remove_cvref_t<ranges::range_reference_t<R>>`. If either `U` is a
  specialization of `std::pair` or `U` is a specialization of `std::tuple` and
  `std::tuple_size_v<U> == 2`, `format_kind<R>` is `range_format::map`.
  Otherwise, `format_kind<R>` is `range_format::set`.
- Otherwise, `format_kind<R>` is `range_format::sequence`.

A program that instantiates a primary template of the `format_kind` variable
template is ill-formed.

User-provided specialization of `format_kind` is allowed as long as:

- `R` is cv-unqualified program-defined type,
- `R` satisfies `input_range`,
- its specialization shall be usable in constant expressions, and
- `format_kind<R>` has type `const range_format`.

### Possible implementation

```cpp
namespace detail
{
    template< typename >
    inline constexpr bool is_pair_or_tuple_2 = false;

    template< typename T, typename U >
    inline constexpr bool is_pair_or_tuple_2<std::pair<T, U>> = true;

    template< typename... Ts >
    inline constexpr bool is_pair_or_tuple_2<std::tuple<Ts...>> = sizeof...(Ts) == 2;

    template < typename T >
        requires std::is_reference_v<T> || std::is_const_v<T>
    inline constexpr bool is_pair_or_tuple_2<T> =
        is_pair_or_tuple_2<std::remove_cvref_t<T>>;
}

template< class R >
inline constexpr range_format format_kind = [] {
    static_assert(false, "instantiating a primary template is not allowed");
    return range_format::disabled;
}();

template< ranges::input_range R >
    requires std::same_as<R, std::remove_cvref_t<R>>
inline constexpr range_format format_kind<R> = [] {
    if constexpr (std::same_as<std::remove_cvref_t<std::ranges::range_reference_t<R>>, R>)
        return range_format::disabled;
    else if constexpr (requires { typename R::key_type; })
    {
        if constexpr (requires { typename R::mapped_type; } &&
                      detail::is_pair_or_tuple_2<std::ranges::range_reference_t<R>>)
            return range_format::map;
        else
            return range_format::set;
    }
    else
        return range_format::sequence;
}();
```

### Example

```cpp
#include <filesystem>
#include <format>
#include <map>
#include <set>
#include <vector>

struct A {};

static_assert(std::format_kind<std::vector<int>> == std::range_format::sequence);
static_assert(std::format_kind<std::map<int>> == std::range_format::map);
static_assert(std::format_kind<std::set<int>> == std::range_format::set);
static_assert(std::format_kind<std::filesystem::path> == std::range_format::disabled);
// ill-formed:
// static_assert(std::format_kind<A> == std::range_format::disabled);

int main() {}
```

### See also

- **range_format (C++23)** — specifies how a range should be formatted (enum)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/format_kind*
