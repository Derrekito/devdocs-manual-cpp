# std::shared_ptr<T>::operator bool

```cpp
explicit operator bool() const noexcept;
```

Checks if `*this` stores a non-null pointer, i.e. whether `get() != nullptr`.

### Parameters

(none)

### Return value

`true` if `*this` stores a pointer, `false` otherwise.

### Notes

An empty shared_ptr (where `use_count() == 0`) may store a non-null pointer
accessible by `get()`, e.g. if it were created using the aliasing constructor.

### Example

```cpp
#include <iostream>
#include <memory>

void report(std::shared_ptr<int> ptr)
{
    if (ptr)
        std::cout << "*ptr=" << *ptr << "\n";
    else
        std::cout << "ptr is not a valid pointer.\n";
}

int main()
{
    std::shared_ptr<int> ptr;
    report(ptr);

    ptr = std::make_shared<int>(7);
    report(ptr);
}
```

Output:

```text
ptr is not a valid pointer.
*ptr=7
```

### See also

- **get** — returns the stored pointer (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator_bool*
