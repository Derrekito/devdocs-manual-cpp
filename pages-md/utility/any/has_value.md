# std::any::has_value

```cpp
bool has_value() const noexcept;  // (since C++17)
```

Checks whether the object contains a value.

### Parameters

(none)

### Return value

`true` if instance contains a value, otherwise `false`.

### Example

```cpp
#include <any>
#include <iostream>
#include <string>

int main()
{
    std::boolalpha(std::cout);

    std::any a0;
    std::cout << "a0.has_value(): " << a0.has_value() << '\n';

    std::any a1 = 42;
    std::cout << "a1.has_value(): " << a1.has_value() << '\n';
    std::cout << "a1 = " << std::any_cast<int>(a1) << '\n';
    a1.reset();
    std::cout << "a1.has_value(): " << a1.has_value() << '\n';

    auto a2 = std::make_any<std::string>("Milky Way");
    std::cout << "a2.has_value(): " << a2.has_value() << '\n';
    std::cout << "a2 = \"" << std::any_cast<std::string&>(a2) << "\"\n";
    a2.reset();
    std::cout << "a2.has_value(): " << a2.has_value() << '\n';
}
```

Output:

```text
a0.has_value(): false
a1.has_value(): true
a1 = 42
a1.has_value(): false
a2.has_value(): true
a2 = "Milky Way"
a2.has_value(): false
```

### See also

- **reset** — destroys contained object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/any/has_value*
