# std::ranges::views::istream, std::ranges::basic_istream_view, std::ranges::istream_view, std::ranges::wistream_view

```cpp
template< std::movable Val, class CharT, class Traits = std::char_traits<CharT> >
    requires std::default_initializable<Val> &&
             /*stream-extractable*/<Val,CharT,Traits>
class basic_istream_view
    : public ranges::view_interface<basic_istream_view<Val,CharT,Traits>>  // (1) (since C++20)
Helper templates
template< class Val >
using istream_view = ranges::basic_istream_view<Val, char>;  // (2) (since C++20)
template< class Val >
using wistream_view = ranges::basic_istream_view<Val, wchar_t>;  // (3) (since C++20)
Customization point objects
namespace views {
    template< class T >
    inline constexpr /*unspecified*/ istream = /*unspecified*/;
}  // (4) (since C++20)
Helper concepts
template< class Val, class CharT, class Traits >
concept /*stream-extractable*/ =
    requires(std::basic_istream<CharT,Traits>& is, Val& t) {
        is >> t;
    };  // (5) (exposition only*)
```

1) A range factory that generates a sequence of elements by repeatedly calling
   `operator>>`.

2,3) Convenience alias templates for character types `char` and `wchar_t`.

4) `views::istream<T>(e)` is expression-equivalent to (has the same effect as)
   `ranges::basic_istream_view<T, typename U::char_type, typename
   U::traits_type>(e)` for any suitable subexpressions `e`, where `U` is
   `std::remove_reference_t<decltype(e)>`. The program is ill-formed if `U` is
   not both publicly and unambiguously derived from `std::basic_istream<typename
   U::char_type, typename U::traits_type>`, which may result in a substitution
   failure.

5) The exposition-only concept `/*stream-extractable*/<Val,CharT,Traits>` is
   satisfied when lvalue of `Val` can be extracted from lvalue of
   `std::basic_istream<CharT,Traits>`.

The iterator type of `basic_istream_view` is move-only: it does not meet the
LegacyIterator requirements, and thus does not work with pre-C++20 algorithms.

### Customization point objects

The name `views::istream<T>` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as
`__istream_fn<T>`.

All instances of `__istream_fn<T>` are equal. The effects of invoking different
instances of type `__istream_fn<T>` on the same arguments are equivalent,
regardless of whether the expression denoting the instance is an lvalue or
rvalue, and is const-qualified or not (however, a volatile-qualified instance is
not required to be invocable). Thus, `views::istream<T>` can be copied freely
and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::istream<T>` above, `__istream_fn<T>`
models

- `std::invocable<__istream_fn<T>, Args...>`,
- `std::invocable<const __istream_fn<T>, Args...>`,
- `std::invocable<__istream_fn<T>&, Args...>`, and
- `std::invocable<const __istream_fn<T>&, Args...>`.

Otherwise, no function call operator of `__istream_fn<T>` participates in
overload resolution.

### Member functions

- **(constructor)** — constructs a `basic_istream_view` (public member function)
- **begin** — returns an iterator (public member function)
- **end** — returns `std::default_sentinel` (public member function)

**Inherited from `std::ranges::view_interface`**

- **(none)** — although `basic_istream_view` is derived from
  `std::ranges::view_interface`, it cannot use any of inherited member
  functions.

## std::ranges::basic_istream_view::basic_istream_view

```cpp
constexpr explicit
    basic_istream_view( std::basic_istream<CharT, Traits>& stream );  // (since C++20)
```

Initializes the stored pointer to stream with `std::addressof(stream)`, and
value-initializes the stored value of `Val`.

## std::ranges::basic_istream_view::begin

```cpp
constexpr auto begin();  // (since C++20)
```

Equivalent to `*stream_ >> value_; return /*iterator*/{*this};`, where `stream_`
is the stored pointer to stream and `value_` is the stored value of `Val`.

## std::ranges::basic_istream_view::end

```cpp
constexpr std::default_sentinel_t end() const noexcept;  // (since C++20)
```

Equivalent to `return std::default_sentinel;`.

### Nested classes

- ***iterator* (C++20)** — the iterator type of `basic_istream_view`
  (exposition-only member class*)

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <ranges>
#include <sstream>
#include <string>

int main()
{
    auto words = std::istringstream{"today is yesterday’s tomorrow"};
    for (const auto& s : std::views::istream<std::string>(words))
        std::cout << std::quoted(s, '/') << ' ';
    std::cout << '\n';

    auto floats = std::istringstream{"1.1  2.2\t3.3\v4.4\f55\n66\r7.7  8.8"};
    std::ranges::copy(
        std::views::istream<float>(floats),
        std::ostream_iterator<float>{std::cout, ", "}
    );
    std::cout << '\n';
}
```

Output:

```text
/today/ /is/ /yesterday’s/ /tomorrow/
1.1, 2.2, 3.3, 4.4, 55, 66, 7.7, 8.8,
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2325R3 | C++20 | default constructor was provided as `view` must be
      `default_initializable` | removed along with the requirement
  LWG 3568 | C++20 | P2325R3 accidentally made the stored value
      default-initialized | restored to value-initialization
  P2432R1 | C++20 | `ranges::istream_view` was a function template and did not
      follow the naming convention | made an alias template; customization point
      objects added

### See also

- **istream_iterator** — input iterator that reads from `std::basic_istream`
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/basic_istream_view*
