# std::uses_allocator<std::tuple>

```cpp
template< class... Types, class Alloc >
struct uses_allocator< std::tuple<Types...>, Alloc > : std::true_type { };  // (since C++11)
```

This specialization of `std::uses_allocator` informs other library components
that tuples support *uses-allocator construction*, even though they do not have
a nested `allocator_type`.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Example

```cpp
// myalloc is a stateful Allocator with a single-argument constructor
// that takes an int. It has no default constructor.

    using innervector_t = std::vector<int, myalloc<int>>;
    using elem_t = std::tuple<int, innervector_t>;
    using Alloc = std::scoped_allocator_adaptor< myalloc<elem_t>, myalloc<int>>;

    Alloc a(1,2);
    std::vector<elem_t, Alloc> v(a);
    v.resize(1);                  // uses allocator #1 for elements of v
    std::get<1>(v[0]).resize(10); // uses allocator #2 for innervector_t
```

### See also

- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/uses_allocator*
