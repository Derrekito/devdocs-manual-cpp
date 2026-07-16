# std::array<T,N>::rbegin, std::array<T,N>::crbegin

```cpp
iterator rbegin() noexcept;  // (1) (since C++11) (constexpr since C++17)
const_iterator rbegin() const noexcept;  // (2) (since C++11) (constexpr since C++17)
const_iterator crbegin() const noexcept;  // (3) (since C++11) (constexpr since C++17)
```

Returns a reverse iterator to the first element of the reversed `array`. It
corresponds to the last element of the non-reversed `array`. If the `array` is
empty, the returned iterator is equal to `rend()`.

### Parameters

(none)

### Return value

Reverse iterator to the first element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <string>
#include <string_view>

int main()
{
    constexpr std::array<std::string_view, 8> data = {"▁","▂","▃","▄","▅","▆","▇","█"};

    std::array<std::string, std::size(data)> arr;

    std::copy(data.cbegin(), data.cend(), arr.begin());
    //             ^              ^           ^

    auto print = [](const std::string_view s) { std::cout << s << ' '; };

    print("Print 'arr' in direct order using [cbegin, cend):\t");
    std::for_each(arr.cbegin(), arr.cend(), print);
    //                ^             ^
    print("\n\nPrint 'arr' in reverse order using [crbegin, crend):\t");
    std::for_each(arr.crbegin(), arr.crend(), print);
    //                ^^             ^^
    print("\n");
}
```

Output:

```text
Print 'arr' in direct order using [cbegin, cend):        ▁ ▂ ▃ ▄ ▅ ▆ ▇ █

Print 'arr' in reverse order using [crbegin, crend):     █ ▇ ▆ ▅ ▄ ▃ ▂ ▁
```

### See also

- **rendcrend** — returns a reverse iterator to the end (public member function)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/array/rbegin*
