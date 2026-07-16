# std::shared_ptr<T>::unique

```cpp
bool unique() const noexcept;  // (deprecated in C++17) (removed in C++20)
```

Checks if `*this` is the only `shared_ptr` instance managing the current object,
i.e. whether `use_count() == 1`.

### Parameters

(none)

### Return value

`true` if `*this` is the only `shared_ptr` instance managing the current object,
`false` otherwise.

### Notes

This function was deprecated in C++17 and removed in C++20 because `use_count`
is only an approximation in multithreaded environment (see Notes in
`use_count`).

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    auto sp1 = std::make_shared<int>(5);
    std::cout << std::boolalpha;
    std::cout << "sp1.unique() == " << sp1.unique() << '\n';

    std::shared_ptr<int> sp2 = sp1;
    std::cout << "sp1.unique() == " << sp1.unique() << '\n';
}
```

Output:

```text
sp1.unique() == true
sp1.unique() == false
```

### See also

- **use_count** — returns the number of `shared_ptr` objects referring to the
  same managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/unique*
