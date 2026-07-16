# std::allocator

```cpp
template< class T >
struct allocator;  // (1)
template<>
struct allocator<void>;  // (2) (deprecated in C++17) (removed in C++20)
```

The `std::allocator` class template is the default Allocator used by all
standard library containers if no user-specified allocator is provided. The
default allocator is stateless, that is, all instances of the given allocator
are interchangeable, compare equal and can deallocate memory allocated by any
other instance of the same allocator type.

The explicit specialization for void lacks the member typedefs `reference`,
`const_reference`, `size_type` and `difference_type`. This specialization
declares no member functions.
*(until C++20)*

The default allocator satisfies allocator completeness requirements.
*(since C++17)*

### Member types

- **`value_type`** — `T`
- **`pointer` (deprecated in C++17)(removed in C++20)** — `T*`
- **`const_pointer` (deprecated in C++17)(removed in C++20)** — const T*
- **`reference` (deprecated in C++17)(removed in C++20)** — `T&`
- **`const_reference` (deprecated in C++17)(removed in C++20)** — const T&
- **`size_type`** — `std::size_t`
- **`difference_type`** — `std::ptrdiff_t`
- **`propagate_on_container_move_assignment` (C++11)** — `std::true_type`
- **`rebind` (deprecated in C++17)(removed in C++20)** — `template< class U >
  struct rebind { typedef allocator<U> other; };`
- **`is_always_equal` (C++11)(deprecated in C++23)(removed in C++26)** —
  `std::true_type`

### Member functions

- **(constructor)** — creates a new allocator instance (public member function)
- **(destructor)** — destructs an allocator instance (public member function)
- **address (until C++20)** — obtains the address of an object, even if
  operator& is overloaded (public member function)
- **allocate** — allocates uninitialized storage (public member function)
- **allocate_at_least (C++23)** — allocates uninitialized storage at least as
  large as requested size (public member function)
- **deallocate** — deallocates storage (public member function)
- **max_size (until C++20)** — returns the largest supported allocation size
  (public member function)
- **construct (until C++20)** — constructs an object in allocated storage
  (public member function)
- **destroy (until C++20)** — destructs an object in allocated storage (public
  member function)

### Non-member functions

- **operator==operator!= (removed in C++20)** — compares two allocator instances
  (public member function)

### Notes

The member template class `rebind` provides a way to obtain an allocator for a
different type. For example, std::list<T, A> allocates nodes of some internal
type `Node<T>`, using the allocator `A::rebind<Node<T>>::other`(until
C++11)`std::allocator_traits<A>::rebind_alloc<Node<T>>`, which is implemented in
terms of `A::rebind<Node<T>>::other` if A is an `std::allocator`(since C++11).

Member type `is_always_equal` is deprecated via LWG issue 3170, because it makes
custom allocators derived from `std::allocator` treated as always equal by
default. std::allocator_traits<std::allocator<T>>::is_always_equal is not
deprecated and its member constant `value` is `true` for any `T`.

### Example

```cpp
#include <iostream>
#include <memory>
#include <string>

int main()
{
    // default allocator for ints
    std::allocator<int> alloc1;

    // demonstrating the few directly usable members
    static_assert(std::is_same_v<int, decltype(alloc1)::value_type>);
    int* p1 = alloc1.allocate(1); // space for one int
    alloc1.deallocate(p1, 1);     // and it is gone

    // Even those can be used through traits though, so no need
    using traits_t1 = std::allocator_traits<decltype(alloc1)>; // The matching trait
    p1 = traits_t1::allocate(alloc1, 1);
    traits_t1::construct(alloc1, p1, 7);  // construct the int
    std::cout << *p1 << '\n';
    traits_t1::deallocate(alloc1, p1, 1); // deallocate space for one int

    // default allocator for strings
    std::allocator<std::string> alloc2;
    // matching traits
    using traits_t2 = std::allocator_traits<decltype(alloc2)>;

    // Rebinding the allocator using the trait for strings gets the same type
    traits_t2::rebind_alloc<std::string> alloc_ = alloc2;

    std::string* p2 = traits_t2::allocate(alloc2, 2); // space for 2 strings

    traits_t2::construct(alloc2, p2, "foo");
    traits_t2::construct(alloc2, p2 + 1, "bar");

    std::cout << p2[0] << ' ' << p2[1] << '\n';

    traits_t2::destroy(alloc2, p2 + 1);
    traits_t2::destroy(alloc2, p2);
    traits_t2::deallocate(alloc2, p2, 2);
}
```

Output:

```text
7
foo bar
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2103 | C++11 | redundant comparison between `allocator` might be required
      | `propagate_on_container_move_assignment` provided
  LWG 2108 | C++11 | there was no way to show `allocator` is stateless |
      `is_always_equal` provided

### See also

- **allocator_traits (C++11)** — provides information about allocator types
  (class template)
- **scoped_allocator_adaptor (C++11)** — implements multi-level allocator for
  multi-level containers (class template)
- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator*
