# std::function<R(Args...)>::assign

```cpp
template< class F, class Alloc >
void assign( F&& f, const Alloc& alloc );  // (since C++11) (removed in C++17)
```

Initializes the *target* with `f`. The `alloc` is used to allocate memory for
any internal data structures that the `function` might use.

Equivalent to `function(std::allocator_arg, alloc,
std::forward<F>(f)).swap(*this);`.

### Parameters

- **f** — callable function to initialize the *target* with
- **alloc** — allocator to use to allocate memory for the internal data
  structures

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### See also

- **operator=** — assigns a new target (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/assign*
