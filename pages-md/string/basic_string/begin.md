# std::basic_string<CharT,Traits,Allocator>::begin, std::basic_string<CharT,Traits,Allocator>::cbegin

```cpp
iterator begin();  // (until C++11)
iterator begin() noexcept;  // (since C++11) (until C++20)
constexpr iterator begin() noexcept;  // (since C++20)
const_iterator begin() const;  // (until C++11)
const_iterator begin() const noexcept;  // (since C++11) (until C++20)
constexpr const_iterator begin() const noexcept;  // (since C++20)
const_iterator cbegin() const;  // (until C++11)
const_iterator cbegin() const noexcept;  // (since C++11) (until C++20)
constexpr const_iterator cbegin() const noexcept;  // (since C++20)
```

Returns an iterator to the first character of the string.

`begin()` returns a mutable or constant iterator, depending on the constness of
`*this`.

`cbegin()` always returns a constant iterator. It is equivalent to
`const_cast<const basic_string&>(*this).begin()`.

### Parameters

(none)

### Return value

Iterator to the first character.

### Complexity

Constant.

### Notes

In libstdc++, `cbegin()` is not available in C++98 mode.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s("Exemplar");
    *s.begin() = 'e';
    std::cout << s << '\n';

    auto i = s.cbegin();
    std::cout << *i << '\n';
//  *i = 'E'; // error: i is a constant iterator
}
```

Output:

```text
exemplar
e
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1192 | C++98 | `std::basic_string` did not have the member function
      `cbegin()` | added

### See also

- **endcend (DR*)** — returns an iterator to the end (public member function)
- **begincbegin** — returns an iterator to the beginning (public member function
  of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/begin*
