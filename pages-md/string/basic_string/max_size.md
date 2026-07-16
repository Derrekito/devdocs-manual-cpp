# std::basic_string<CharT,Traits,Allocator>::max_size

```cpp
size_type max_size() const;  // (until C++11)
size_type max_size() const noexcept;  // (since C++11) (until C++20)
constexpr size_type max_size() const noexcept;  // (since C++20)
```

Returns the maximum number of elements the string is able to hold due to system
or library implementation limitations, i.e. `std::distance(begin(), end())` for
the largest string.

### Parameters

(none)

### Return value

Maximum number of characters.

### Complexity

Constant.

### Example

```cpp
#include <array>
#include <climits>
#include <iomanip>
#include <iostream>
#include <locale>
#include <string>
#include <typeinfo>

#include <boost/core/demangle.hpp>

template<typename T>
void print_basic_string_max_size()
{
    std::basic_string<T> s;
    auto max_size = s.max_size();
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "basic_string<" << boost::core::demangle(typeid(T).name())
              << ">:\n\t" << max_size << " = ";
    std::cout << std::setprecision(3) << (double) max_size << " = ";
    std::cout.imbue(std::locale("C"));
    std::cout << std::hex << std::setfill('0') << "0x"
              << std::setw(sizeof(typename decltype(s)::size_type) << 1)
              << max_size << '\n' << std::dec;
};

int main()
{
    std::cout << "Pointer size: " << CHAR_BIT * sizeof(void*) << " bits\n"
                 "Maximum sizes:\n";

    print_basic_string_max_size<char>();
    print_basic_string_max_size<char16_t>();
    print_basic_string_max_size<char32_t>();
    print_basic_string_max_size<wchar_t>();
    print_basic_string_max_size<long>();

    using CharT = std::array<char, 01232>;
    print_basic_string_max_size<CharT>();
}
```

Possible output:

```text
Pointer size: 64 bits
Maximum sizes:
basic_string<char>:
        9,223,372,036,854,775,807 = 9.22e+18 = 0x7fffffffffffffff
basic_string<char16_t>:
        4,611,686,018,427,387,903 = 4.61e+18 = 0x3fffffffffffffff
basic_string<char32_t>:
        2,305,843,009,213,693,951 = 2.31e+18 = 0x1fffffffffffffff
basic_string<wchar_t>:
        2,305,843,009,213,693,951 = 2.31e+18 = 0x1fffffffffffffff
basic_string<long>:
        1,152,921,504,606,846,975 = 1.15e+18 = 0x0fffffffffffffff
basic_string<std::array<char, 666ul>>:
        13,848,906,962,244,407 = 1.38e+16 = 0x00313381ec031337
```

### See also

- **sizelength** — returns the number of characters (public member function)
- **max_size** — returns the maximum number of characters (public member
  function of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/max_size*
