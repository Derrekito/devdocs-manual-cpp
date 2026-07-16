# std::shared_ptr<T>::operator<<

```cpp
template< class T, class U, class V >
std::basic_ostream<U, V>& operator<<( std::basic_ostream<U, V>& os, const std::shared_ptr<T>& ptr );
```

Inserts the value of the pointer stored in `ptr` into the output stream `os`.

Equivalent to `os << ptr.get()`.

### Parameters

- **os** — a `std::basic_ostream` to insert `ptr` into
- **ptr** — the data to be inserted into `os`

### Return value

`os`

### Example

```cpp
#include <iostream>
#include <memory>

class Foo {};

int main()
{
    auto sp = std::make_shared<Foo>();
    std::cout << sp << '\n';
    std::cout << sp.get() << '\n';
}
```

Possible output:

```text
0x6d9028
0x6d9028
```

### See also

- **get** — returns the stored pointer (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator_ltlt*
