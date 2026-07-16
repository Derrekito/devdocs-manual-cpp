# std::list<T,Allocator>::push_back

```cpp
void push_back( const T& value );  // (1)
void push_back( T&& value );  // (2) (since C++11)
```

Appends the given element `value` to the end of the container.

1) The new element is initialized as a copy of `value`.

2) `value` is moved into the new element.

No iterators or references are invalidated.

### Parameters

- **value** — the value of the element to append

**Type requirements**

**-`T` must meet the requirements of CopyInsertable in order to use overload (1).**

**-`T` must meet the requirements of MoveInsertable in order to use overload (2).**

### Return value

(none)

### Complexity

Constant.

### Exceptions

If an exception is thrown (which can be due to `Allocator::allocate()` or
element copy/move constructor/assignment), this function has no effect (strong
exception guarantee).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <list>

int main()
{
    std::list<std::string> letters;

    letters.push_back("abc");
    std::string s{"def"};
    letters.push_back(std::move(s));

    std::cout << "std::list letters holds: ";
    for (auto&& e : letters)
        std::cout << std::quoted(e) << ' ';

    std::cout << "\nMoved-from string s holds: " << std::quoted(s) << '\n';
}
```

Possible output:

```text
std::list letters holds: "abc" "def"
Moved-from string s holds: ""
```

### See also

- **emplace_back (C++11)** — constructs an element in-place at the end (public
  member function)
- **push_front** — inserts an element to the beginning (public member function)
- **pop_back** — removes the last element (public member function)
- **back_inserter** — creates a `std::back_insert_iterator` of type inferred
  from the argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/list/push_back*
