# std::deque<T,Allocator>::push_front

```cpp
void push_front( const T& value );  // (1)
void push_front( T&& value );  // (2) (since C++11)
```

Prepends the given element `value` to the beginning of the container.

All iterators (including the `end()` iterator) are invalidated. No references
are invalidated.

### Parameters

- **value** — the value of the element to prepend

### Return value

(none)

### Complexity

Constant.

### Exceptions

If an exception is thrown for any reason, these functions have no effect (strong
exception safety guarantee).

### Example

```cpp
#include <deque>
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::deque<std::string> letters;

    letters.push_front("abc");
    std::string s{"def"};
    letters.push_front(std::move(s));

    std::cout << "std::deque letters holds: ";
    for (auto&& e : letters)
        std::cout << std::quoted(e) << ' ';

    std::cout << "\nMoved-from string s holds: " << std::quoted(s) << '\n';
}
```

Possible output:

```text
std::deque letters holds: "def" "abc"
Moved-from string s holds: ""
```

### See also

- **emplace_front (C++11)** — constructs an element in-place at the beginning
  (public member function)
- **push_back** — adds an element to the end (public member function)
- **pop_front** — removes the first element (public member function)
- **front_inserter** — creates a `std::front_insert_iterator` of type inferred
  from the argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/push_front*
