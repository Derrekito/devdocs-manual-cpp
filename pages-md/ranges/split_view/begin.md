# std::ranges::split_view<V,Pattern>::begin

```cpp
constexpr /*iterator*/ begin();  // (since C++20)
```

Returns an `iterator` to the first found subrange.

In order to provide the amortized constant time complexity required by the
`range` concept, this function caches the result within the `split_view` for use
on subsequent calls. Equivalent to

```cpp
constexpr /*iterator*/ begin()
{
    if (!cached_begin_.has_value())
        cached_begin_ = this->find_next(ranges::begin(base_));
    return {*this, ranges::begin(base_), cached_begin_.value()};
}
```

Exposition only data members `base_` and `cached_begin_` are described here.

### Parameters

(none)

### Return value

An `iterator`.

### Complexity

Amortized \(\scriptsize \mathcal{O}(1)\)O(1).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <ranges>
#include <string_view>

int main()
{
    constexpr std::string_view sentence{"Keep..moving..forward.."};
    constexpr std::string_view delim{".."};
    std::ranges::split_view words{sentence, delim};

    std::cout << "begin(): " << std::quoted(std::string_view{*words.begin()})
              << "\nSubstrings: ";
    for (std::string_view word : words)
        std::cout << std::quoted(word) << ' ';

    std::ranges::split_view letters{sentence, std::string_view{""}};
    std::cout << "\nbegin(): " << std::quoted(std::string_view{*letters.begin()})
              << "\nLetters: ";
    for (std::string_view letter : letters)
        std::cout << letter << ' ';
    std::cout << '\n';
}
```

Output:

```text
begin(): "Keep"
Substrings: "Keep" "moving" "forward" ""
begin(): "K"
Letters: K e e p . . m o v i n g . . f o r w a r d . .
```

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function of `std::ranges::lazy_split_view<V,Pattern>`)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view/begin*
