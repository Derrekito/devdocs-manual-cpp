# std::unique_ptr<T,Deleter>::operator<<

```cpp
template< class CharT, class Traits, class Y, class D >
std::basic_ostream<CharT, Traits>& operator<<( std::basic_ostream<CharT, Traits>& os,
                                               const std::unique_ptr<Y, D>& p );  // (since C++20)
```

Inserts the value of the pointer managed by `p` into the output stream `os`.

Equivalent to `os << p.get()`.

This overload participates in overload resolution only if `os << p.get()` is a
valid expression.

### Parameters

- **os** — a `std::basic_ostream` to insert `p` into
- **p** — the pointer to be inserted into `os`

### Return value

`os`

### Notes

If `std::unique_ptr<Y, D>::pointer` is a pointer to a character type (e.g., when
`Y` is `char`(`[]`) or `CharT`(`[]`)), this may end up calling the overloads of
`operator<<` for null-terminated character strings (causing undefined behavior
if the pointer does not in fact point to such a string), rather than the
overload for printing the value of the pointer itself.

### Example

```cpp
#include <iostream>
#include <memory>

class Foo {};

int main()
{
    auto p = std::make_unique<Foo>();
    std::cout << p << '\n';
    std::cout << p.get() << '\n';
}
```

Possible output:

```text
0x6d9028
0x6d9028
```

### See also

- **get** — returns a pointer to the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/operator_ltlt*
