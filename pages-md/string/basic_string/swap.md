# std::basic_string<CharT,Traits,Allocator>::swap

```cpp
void swap( basic_string& other );  // (until C++17)
void swap( basic_string& other ) noexcept(/* see below */);  // (since C++17) (until C++20)
constexpr void swap( basic_string& other ) noexcept(/* see below */);  // (since C++20)
```

Exchanges the contents of the string with those of `other`. All iterators and
references may be invalidated.

The behavior is undefined if `Allocator` does not propagate on swap and the
allocators of `*this` and `other` are unequal.
*(since C++11)*

### Parameters

- **other** — string to exchange the contents with

### Return value

(none)

### Complexity

Constant.

### Exceptions

No exception is thrown.
*(until C++11)*

Exceptions can only be thrown in the case where the behavior is undefined (see
above).
If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).
*(since C++11)*

`noexcept` specification:
`noexcept(std::allocator_traits<Allocator>::propagate_on_container_swap::value
|| std::allocator_traits<Allocator>::is_always_equal::value)`
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

    a.swap(b);

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
  LWG 403 | C++98 | `swap()` might throw an exception | no exception is thrown
  LWG 535 | C++98 | swapping strings did not preserve the character orders |
      orders are also preserved

### See also

- **swap** — swaps the values of two objects (function template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **swap** — swaps the contents (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/swap*
