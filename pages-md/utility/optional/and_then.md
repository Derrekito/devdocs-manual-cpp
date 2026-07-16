# std::optional<T>::and_then

```cpp
template< class F >
constexpr auto and_then( F&& f ) &;  // (1) (since C++23)
template< class F >
constexpr auto and_then( F&& f ) const&;  // (2) (since C++23)
template< class F >
constexpr auto and_then( F&& f ) &&;  // (3) (since C++23)
template< class F >
constexpr auto and_then( F&& f ) const&&;  // (4) (since C++23)
```

If `*this` contains a value, invokes `f` with the contained value as an
argument, and returns the result of that invocation; otherwise, returns an empty
`std::optional`.

The return type (see below) must be a specialization of `std::optional` (unlike
`transform()`). Otherwise, the program is ill-formed.

1) Equivalent to if (*this) return std::invoke(std::forward<F>(f), **this); else
   return std::remove_cvref_t<std::invoke_result_t<F, T&>>{};

2) Equivalent to if (*this) return std::invoke(std::forward<F>(f), **this); else
   return std::remove_cvref_t<std::invoke_result_t<F, const T&>>{};

3) Equivalent to if (*this) return std::invoke(std::forward<F>(f),
   std::move(**this)); else return std::remove_cvref_t<std::invoke_result_t<F,
   T>>{};

4) Equivalent to if (*this) return std::invoke(std::forward<F>(f),
   std::move(**this)); else return std::remove_cvref_t<std::invoke_result_t<F,
   const T>>{};

### Parameters

- **f** — a suitable function or Callable object that returns an `std::optional`

### Return value

The result of `f` or an empty `std::optional`, as described above.

### Notes

Some languages call this operation *flatmap*.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_optional` | 202110L | (C++23) | Monadic operations in
      `std::optional`

### Example

```cpp
#include <charconv>
#include <iomanip>
#include <iostream>
#include <optional>
#include <ranges>
#include <string>
#include <string_view>
#include <vector>

std::optional<int> to_int(std::string_view sv)
{
    int r{};
    auto [ptr, ec]{std::from_chars(sv.data(), sv.data() + sv.size(), r)};
    if (ec == std::errc())
        return r;
    else
        return std::nullopt;
}

int main()
{
    using namespace std::literals;

    const std::vector<std::optional<std::string>> v
    {
        "1234", "15 foo", "bar", "42", "5000000000", " 5", std::nullopt, "-43"
    };

    for (auto&& x : v | std::views::transform(
        [](auto&& o)
        {
            // debug print the content of input optional<string>
            std::cout << std::left << std::setw(13)
                      << std::quoted(o.value_or("nullopt")) << " -> ";

            return o
                // if optional is nullopt convert it to optional with "" string
                .or_else([]{ return std::optional{""s}; })
                // flatmap from strings to ints (making empty optionals where it fails)
                .and_then(to_int)
                // map int to int + 1
                .transform([](int n) { return n + 1; })
                // convert back to strings
                .transform([](int n) { return std::to_string(n); })
                // replace all empty optionals that were left by
                // and_then and ignored by transforms with "NaN"
                .value_or("NaN"s);
        }))
        std::cout << x << '\n';
}
```

Output:

```text
"1234"        -> 1235
"15 foo"      -> 16
"bar"         -> NaN
"42"          -> 43
"5000000000"  -> NaN
" 5"          -> NaN
"nullopt"     -> NaN
"-43"         -> -42
```

### See also

- **value_or** — returns the contained value if available, another value
  otherwise (public member function)
- **transform (C++23)** — returns an `optional` containing the transformed
  contained value if it exists, or an empty `optional` otherwise (public member
  function)
- **or_else (C++23)** — returns the `optional` itself if it contains a value, or
  the result of the given function otherwise (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/and_then*
