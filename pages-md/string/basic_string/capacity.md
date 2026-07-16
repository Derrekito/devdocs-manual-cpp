# std::basic_string<CharT,Traits,Allocator>::capacity

```cpp
size_type capacity() const;  // (until C++11)
size_type capacity() const noexcept;  // (since C++11) (until C++20)
constexpr size_type capacity() const noexcept;  // (since C++20)
```

Returns the number of characters that the string has currently allocated space
for.

### Parameters

(none)

### Return value

Capacity of the currently allocated storage, i.e. the storage available for
storing elements.

### Complexity

Constant.

### Notes

Memory locations obtained from the allocator but not available for storing any
element are not counted in the allocated storage. Note that the null terminator
is not an element of the `std::basic_string`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>

void show_capacity(std::string const& s)
{
    std::cout << std::quoted(s) << " has capacity " << s.capacity() << ".\n";
}

int main()
{
    std::string s{"Exemplar"};
    show_capacity(s);

    s += " is an example string.";
    show_capacity(s);

    s.clear();
    show_capacity(s);

    std::cout << "\nDemonstrate the capacity's growth policy."
                 "\nSize:  Capacity:  Ratio:\n" << std::left;

    std::string g;
    auto old_cap{g.capacity()};

    for (int mark{}; mark != 5; ++mark)
    {
        while (old_cap == g.capacity())
            g.push_back('.');

        std::cout << std::setw( 7) << g.size()
                  << std::setw(11) << g.capacity()
                  << std::setw(10) << g.capacity() / static_cast<float>(old_cap) << '\n';

        old_cap = g.capacity();
    }
}
```

Possible output:

```text
"Exemplar" has capacity 15.
"Exemplar is an example string." has capacity 30.
"" has capacity 30.

Demonstrate the capacity's growth policy.
Size:  Capacity:  Ratio:
16     30         2
31     60         2
61     120        2
121    240        2
241    480        2
```

### See also

- **sizelength** — returns the number of characters (public member function)
- **reserve** — reserves storage (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/capacity*
