# std::unique_ptr<T,Deleter>::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++11) (constexpr since C++23)
```

Checks whether `*this` owns an object, i.e. whether `get()` `!=` `nullptr`.

### Parameters

(none)

### Return value

`true` if `*this` owns an object, `false` otherwise.

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::unique_ptr<int> ptr(new int(42));

    if (ptr)
        std::cout << "before reset, ptr is: " << *ptr << '\n';
    ptr.reset();
    ptr ? (std::cout << "after reset, ptr is: " << *ptr)
        : (std::cout << "after reset ptr is empty") << '\n';
}
```

Output:

```text
before reset, ptr is: 42
after reset ptr is empty
```

### See also

- **get** — returns a pointer to the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/operator_bool*
