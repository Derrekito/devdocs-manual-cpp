# std::system_error::operator=

```cpp
system_error& operator=( const system_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::system_error` then `std::strcmp(what(), other.what()) == 0`
after assignment.

### Parameters

- **other** — another `system_error` object to assign with

### Return value

`*this`

### Example

```cpp
#include <cassert>
#include <cstring>
#include <iostream>
#include <system_error>

void print(const std::system_error& e)
{
    std::cout << "code:    [" << e.code() << "]\n"
                 "message: [" << e.code().message() << "]\n"
                 "what:    [" << e.what() << "]\n\n";
}

int main()
{
    std::system_error e1(EDOM, std::generic_category(), "Error info #1");
    print(e1);

    std::system_error e2(EIO, std::system_category(), "Error info #2");
    print(e2);

    e1 = e2;
    assert(std::strcmp(e1.what(), e2.what()) == 0);
    print(e1);
}
```

Possible output:

```text
code:    [generic:33]
message: [Numerical argument out of domain]
what:    [Error info #1: Numerical argument out of domain]

code:    [system:5]
message: [Input/output error]
what:    [Error info #2: Input/output error]

code:    [system:5]
message: [Input/output error]
what:    [Error info #2: Input/output error]
```

---
*Source: https://en.cppreference.com/w/cpp/error/system_error/operator%3D*
