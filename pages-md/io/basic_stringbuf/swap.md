# std::basic_stringbuf<CharT,Traits,Allocator>::swap

```cpp
void swap( basic_stringbuf& rhs );  // (since C++11) (until C++20)
void swap( basic_stringbuf& rhs ) noexcept(/* see below */);  // (since C++20)
```

Swaps the state and the contents of `*this` and `rhs`.

The behavior is undefined if `Allocator` does not propagate on swap and the
allocators of `*this` and `other` are unequal.
*(since C++11)*

### Parameters

- **rhs** — another `basic_stringbuf`

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.
*(since C++11)(until C++20)*

`noexcept` specification:
`noexcept(std::allocator_traits<Allocator>::propagate_on_container_swap::value
|| std::allocator_traits<Allocator>::is_always_equal::value)`
*(since C++20)*

### Notes

This function is called automatically when swapping `std::stringstream` objects.
It is rarely necessary to call it directly.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::istringstream one("one");
    std::ostringstream two("two");

    std::cout << "Before swap: one = " << std::quoted(one.str())
              << ", two = " << std::quoted(two.str()) << ".\n";

    one.rdbuf()->swap(*two.rdbuf());

    std::cout << "After  swap: one = " << std::quoted(one.str())
              << ", two = " << std::quoted(two.str()) << ".\n";
}
```

Output:

```text
Before swap: one = "one", two = "two".
After  swap: one = "two", two = "one".
```

### See also

- **(constructor)** — constructs a `basic_stringbuf` object (public member
  function)
- **swap (C++11)** — swaps two string streams (public member function of
  `std::basic_stringstream<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringbuf/swap*
