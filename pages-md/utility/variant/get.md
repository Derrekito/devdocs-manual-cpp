# std::get (std::variant)

```cpp
template< std::size_t I, class... Types >
constexpr std::variant_alternative_t<I, std::variant<Types...>>&
    get( std::variant<Types...>& v );
template< std::size_t I, class... Types >
constexpr std::variant_alternative_t<I, std::variant<Types...>>&&
    get( std::variant<Types...>&& v );
template< std::size_t I, class... Types >
constexpr const std::variant_alternative_t<I, std::variant<Types...>>&
    get( const std::variant<Types...>& v );
template< std::size_t I, class... Types >
constexpr const std::variant_alternative_t<I, std::variant<Types...>>&&
    get( const std::variant<Types...>&& v );
template< class T, class... Types >
constexpr T& get( std::variant<Types...>& v );
template< class T, class... Types >
constexpr T&& get( std::variant<Types...>&& v );
template< class T, class... Types >
constexpr const T& get( const std::variant<Types...>& v );
template< class T, class... Types >
constexpr const T&& get( const std::variant<Types...>&& v );
```

1) Index-based value accessor: If `v.index() == I`, returns a reference to the
   value stored in `v`. Otherwise, throws `std::bad_variant_access`. The call is
   ill-formed if `I` is not a valid index in the variant.

2) Type-based value accessor: If `v` holds the alternative `T`, returns a
   reference to the value stored in `v`. Otherwise, throws
   `std::bad_variant_access`. The call is ill-formed if `T` is not a unique
   element of `Types...`.

### Template parameters

- **I** — index to look up
- **T** — unique type to look up
- **Types...** — types forming the `variant`

### Parameters

- **v** — a `variant`

### Return value

Reference to the value stored in the variant.

### Exceptions

1,2) Throws `std::bad_variant_access` on errors.

### Example

```cpp
#include <variant>
#include <string>
#include <iostream>

int main()
{
    std::variant<int, float> v{12}, w;
    std::cout << std::get<int>(v) << '\n';
    w = std::get<int>(v);
    w = std::get<0>(v); // same effect as the previous line

//  std::get<double>(v); // error: no double in [int, float]
//  std::get<3>(v);      // error: valid index values are 0 and 1

    try
    {
        w = 42.0f;
        std::cout << std::get<float>(w) << '\n'; // ok, prints 42
        w = 42;
        std::cout << std::get<float>(w) << '\n'; // throws
    }
    catch (std::bad_variant_access const& ex)
    {
        std::cout << ex.what() << ": w contained int, not float\n";
    }
}
```

Possible output:

```text
12
42
Unexpected index: w contained int, not float
```

### See also

- **get_if (C++17)** — obtains a pointer to the value of a pointed-to variant
  given the index or the type (if unique), returns null on error (function
  template)
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)
- **get(std::ranges::subrange) (C++20)** — obtains iterator or sentinel from a
  `std::ranges::subrange` (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/get*
