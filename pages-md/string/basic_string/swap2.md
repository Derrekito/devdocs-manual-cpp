# std::swap(std::basic_string)

```cpp
template< class CharT, class Traits, class Alloc >
void swap( std::basic_string<CharT, Traits, Alloc>& lhs,
           std::basic_string<CharT, Traits, Alloc>& rhs );  // (until C++17)
template< class CharT, class Traits, class Alloc >
void swap( std::basic_string<CharT, Traits, Alloc>& lhs,
           std::basic_string<CharT, Traits, Alloc>& rhs ) noexcept(/* see below */);  // (since C++17) (until C++20)
template< class CharT, class Traits, class Alloc >
constexpr void
    swap( std::basic_string<CharT, Traits, Alloc>& lhs,
          std::basic_string<CharT, Traits, Alloc>& rhs ) noexcept(/* see below */);  // (since C++20)
```

Specializes the `std::swap` algorithm for `std::basic_string`. Swaps the
contents of `lhs` and `rhs`.

Equivalent to `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — strings whose contents to swap

### Return value

(none)

### Complexity

Constant.

### Exceptions
`noexcept` specification:
`noexcept(noexcept(lhs.swap(rhs)))`
*(since C++17)*

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string a = "AAA";
    std::string b = "BBBB";

    std::cout << "Before swap:\n"
                 "a = " << a << "\n"
                 "b = " << b << "\n\n";

    std::swap(a, b);

    std::cout << "After swap:\n"
                 "a = " << a << "\n"
                 "b = " << b << '\n';
}
```

Output:

```text
Before swap:
a = AAA
b = BBBB

After swap:
a = BBBB
b = AAA
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2064 | C++11 | non-member `swap` was noexcept and inconsistent with member
      `swap` | noexcept removed

### See also

- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/swap2*
