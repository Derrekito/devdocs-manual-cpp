# std::vector<T,Allocator>::push_back

```cpp
void push_back( const T& value );  // (until C++20)
constexpr void push_back( const T& value );  // (since C++20)
void push_back( T&& value );  // (since C++11) (until C++20)
constexpr void push_back( T&& value );  // (since C++20)
```

Appends the given element `value` to the end of the container.

1) The new element is initialized as a copy of `value`.

2) `value` is moved into the new element.

If after the operation the new `size()` is greater than old `capacity()` a
reallocation takes place, in which case all iterators (including the `end()`
iterator) and all references to the elements are invalidated. Otherwise only the
`end()` iterator is invalidated.

### Parameters

- **value** — the value of the element to append

**Type requirements**

**-`T` must meet the requirements of CopyInsertable in order to use overload (1).**

**-`T` must meet the requirements of MoveInsertable in order to use overload (2).**

### Return value

(none)

### Complexity

Amortized constant.

### Exceptions

If an exception is thrown (which can be due to `Allocator::allocate()` or
element copy/move constructor/assignment), this function has no effect (strong
exception guarantee).

If `T`'s move constructor is not `noexcept` and `T` is not CopyInsertable into
`*this`, vector will use the throwing move constructor. If it throws, the
guarantee is waived and the effects are unspecified.
*(since C++11)*

### Notes

Some implementations throw `std::length_error` when `push_back` causes a
reallocation that exceeds `max_size` (due to an implicit call to an equivalent
of `reserve``(``size``() + 1))`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <vector>

int main()
{
    std::vector<std::string> letters;

    letters.push_back("abc");
    std::string s{"def"};
    letters.push_back(std::move(s));

    std::cout << "std::vector letters holds: ";
    for (auto&& e : letters)
        std::cout << std::quoted(e) << ' ';

    std::cout << "\nMoved-from string s holds: " << std::quoted(s) << '\n';
}
```

Possible output:

```text
std::vector letters holds: "abc" "def"
Moved-from string s holds: ""
```

### See also

- **emplace_back (C++11)** — constructs an element in-place at the end (public
  member function)
- **pop_back** — removes the last element (public member function)
- **back_inserter** — creates a `std::back_insert_iterator` of type inferred
  from the argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/push_back*
