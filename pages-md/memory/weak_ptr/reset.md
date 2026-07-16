# std::weak_ptr<T>::reset

```cpp
void reset() noexcept;  // (since C++11)
```

Releases the reference to the managed object. After the call `*this` manages no
object.

### Parameters

(none)

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    auto shared = std::make_shared<int>(), shared2 = shared, shared3 = shared2;

    auto weak = std::weak_ptr<int>{shared};

    std::cout << std::boolalpha
              << "shared.use_count(): " << shared.use_count() << '\n'
              << "weak.use_count(): " << weak.use_count() << '\n'
              << "weak.expired(): " << weak.expired() << '\n';

    weak.reset();

    std::cout << "weak.reset();\n"
              << "shared.use_count(): " << shared.use_count() << '\n'
              << "weak.use_count(): " << weak.use_count() << '\n'
              << "weak.expired(): " << weak.expired() << '\n';
}
```

Output:

```text
shared.use_count(): 3
weak.use_count(): 3
weak.expired(): false
weak.reset();
shared.use_count(): 3
weak.use_count(): 0
weak.expired(): true
```

### See also

- **expired** — checks whether the referenced object was already deleted (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr/reset*
