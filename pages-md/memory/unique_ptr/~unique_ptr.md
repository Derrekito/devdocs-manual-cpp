# std::unique_ptr<T,Deleter>::~unique_ptr

```cpp
~unique_ptr();  // (since C++11) (constexpr since C++23)
```

If `get()` `==` `nullptr` there are no effects. Otherwise, the owned object is
destroyed via `get_deleter()``(``get()``)`.

Requires that `get_deleter()(get())` does not throw exceptions.

### Notes

Although `std::unique_ptr<T>` with the default deleter may be constructed with
incomplete type `T`, the type `T` must be complete at the point of code where
the destructor is called.

### Example

The following program demonstrates usage of a custom deleter.

```cpp
#include <iostream>
#include <memory>

int main ()
{
    auto deleter = [](int* ptr)
    {
        std::cout << "[deleter called]\n";
        delete ptr;
    };

    std::unique_ptr<int, decltype(deleter)> uniq(new int, deleter);
    std::cout << (uniq ? "not empty\n" : "empty\n");
    uniq.reset();
    std::cout << (uniq ? "not empty\n" : "empty\n");
}
```

Output:

```text
not empty
[deleter called]
empty
```

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/%7Eunique_ptr*
