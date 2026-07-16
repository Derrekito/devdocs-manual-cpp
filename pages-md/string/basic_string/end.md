# std::basic_string<CharT,Traits,Allocator>::end, std::basic_string<CharT,Traits,Allocator>::cend

```cpp
iterator end();  // (until C++11)
iterator end() noexcept;  // (since C++11) (until C++20)
constexpr iterator end() noexcept;  // (since C++20)
const_iterator end() const;  // (until C++11)
const_iterator end() const noexcept;  // (since C++11) (until C++20)
constexpr const_iterator end() const noexcept;  // (since C++20)
const_iterator cend() const;  // (until C++11)
const_iterator cend() const noexcept;  // (since C++11) (until C++20)
constexpr const_iterator cend() const noexcept;  // (since C++20)
```

Returns an iterator to the character following the last character of the string.
This character acts as a placeholder, attempting to access it results in
undefined behavior.

### Parameters

(none)

### Return value

Iterator to the character following the last character.

### Complexity

Constant.

### Notes

In libstdc++, `cend()` is not available in C++98 mode.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string s("Exemparl");
    std::next_permutation(s.begin(), s.end());

    std::string c;
    std::copy(s.cbegin(), s.cend(), std::back_inserter(c));
    std::cout << c << '\n'; // "Exemplar"
}
```

Output:

```text
Exemplar
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1192 | C++98 | `std::basic_string` did not have the member function
      `cend()` | added

### See also

- **begincbegin (DR*)** — returns an iterator to the beginning (public member
  function)
- **endcend** — returns an iterator to the end (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/end*
