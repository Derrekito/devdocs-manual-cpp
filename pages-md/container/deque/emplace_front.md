# std::deque<T,Allocator>::emplace_front

```cpp
template< class... Args >
void emplace_front( Args&&... args );  // (since C++11) (until C++17)
template< class... Args >
reference emplace_front( Args&&... args );  // (since C++17)
```

Inserts a new element to the beginning of the container. The element is
constructed through `std::allocator_traits::construct`, which typically uses
placement-new to construct the element in-place at the location provided by the
container. The arguments `args...` are forwarded to the constructor as
`std::forward<Args>(args)...`.

All iterators (including the `end()` iterator) are invalidated. No references
are invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

**Type requirements**

**-`T (the container's element type)` must meet the requirements of EmplaceConstructible.**

### Return value

(none)
*(until C++17)*

A reference to the inserted element.
*(since C++17)*

### Complexity

Constant.

### Exceptions

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### See also

- **push_front** — inserts an element to the beginning (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/emplace_front*
