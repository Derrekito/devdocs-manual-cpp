# std::scoped_allocator_adaptor

```cpp
template< class OuterAlloc, class... InnerAllocs >
class scoped_allocator_adaptor
    : public OuterAlloc;  // (since C++11)
```

The `std::scoped_allocator_adaptor` class template is an allocator which can be
used with multilevel containers (vector of sets of lists of tuples of maps,
etc). It is instantiated with one outer allocator type `OuterAlloc` and zero or
more inner allocator types `InnerAlloc...`. A container constructed directly
with a `scoped_allocator_adaptor` uses `OuterAlloc` to allocate its elements,
but if an element is itself a container, it uses the first inner allocator. The
elements of that container, if they are themselves containers, use the second
inner allocator, etc. If there are more levels to the container than there are
inner allocators, the last inner allocator is reused for all further nested
containers.

The purpose of this adaptor is to correctly initialize stateful allocators in
nested containers, such as when all levels of a nested container must be placed
in the same shared memory segment. The adaptor's constructor takes the arguments
for all allocators in the list, and each nested container obtains its
allocator's state from the adaptor as needed.

For the purpose of `scoped_allocator_adaptor`, if the next inner allocator is
`A`, any class `T` for which `std::uses_allocator<T,A>::value == true`
participates in the recursion as if it was a container. Additionally,
`std::pair` is treated as such a container by specific overloads of
`scoped_allocator_adaptor::construct`.

Typical implementation holds an instance of a
`std::scoped_allocator_adaptor<InnerAllocs...>` as a member object.

Note that `std::pmr::polymorphic allocator`s propagate to nested containers
following uses-allocator construction and do not need (and do not work with)
`std::scoped_allocator_adaptor`.

### Member types

- **`outer_allocator_type`** — `OuterAlloc`
- **`inner_allocator_type`** — `scoped_allocator_adaptor<InnerAllocs...>` or, if
  `sizeof...(InnerAllocs) == 0`, `scoped_allocator_adaptor<OuterAlloc>`
- **`value_type`** — `std::allocator_traits<OuterAlloc>::value_type`
- **`size_type`** — `std::allocator_traits<OuterAlloc>::size_type`
- **`difference_type`** — `std::allocator_traits<OuterAlloc>::difference_type`
- **`pointer`** — `std::allocator_traits<OuterAlloc>::pointer`
- **`const_pointer`** — `std::allocator_traits<OuterAlloc>::const_pointer`
- **`void_pointer`** — `std::allocator_traits<OuterAlloc>::void_pointer`
- **`const_void_pointer`** —
  `std::allocator_traits<OuterAlloc>::const_void_pointer`

**`propagate_on_container_copy_assignment` `std::true_type` if `std::allocator_traits<A>::propagate_on_container_copy_assignment::value` is `true` for at least one allocator `A` among `OuterAlloc` and `InnerAlloc...`**

**`propagate_on_container_move_assignment` `std::true_type` if `std::allocator_traits<A>::propagate_on_container_move_assignment::value` is `true` for at least one allocator `A` among `OuterAlloc` and `InnerAlloc...`**

**`propagate_on_container_swap` `std::true_type` if `std::allocator_traits<A>::propagate_on_container_swap::value` is `true` for at least one allocator `A` among `OuterAlloc` and `InnerAlloc...`**

**`is_always_equal` `std::true_type` if `std::allocator_traits<A>::is_always_equal::value` is `true` for every allocator `A` among `OuterAlloc` and `InnerAlloc...`**

**`rebind` `template< class T > struct rebind { typedef scoped_allocator_adaptor< std::allocator_traits<OuterAlloc>::template rebind_alloc<T>, InnerAllocs... > other; };`**

### Member functions

- **(constructor)** — creates a new scoped_allocator_adaptor instance (public
  member function)
- **(destructor)** — destructs a `scoped_allocator_adaptor` instance (public
  member function)
- **operator=** — assigns a `scoped_allocator_adaptor` (public member function)
- **inner_allocator** — obtains an `inner_allocator` reference (public member
  function)
- **outer_allocator** — obtains an `outer_allocator` reference (public member
  function)
- **allocate** — allocates uninitialized storage using the outer allocator
  (public member function)
- **deallocate** — deallocates storage using the outer allocator (public member
  function)
- **max_size** — returns the largest allocation size supported by the outer
  allocator (public member function)
- **construct** — constructs an object in allocated storage, passing the inner
  allocator to its constructor if appropriate (public member function)
- **destroy** — destructs an object in allocated storage (public member
  function)
- **select_on_container_copy_construction** — copies the state of
  `scoped_allocator_adaptor` and all its allocators (public member function)

### Non-member functions

- **operator==operator!= (removed in C++20)** — compares two
  scoped_allocator_adaptor instances (function template)

### Deduction guides(since C++17)

### Example

```cpp
#include <boost/interprocess/allocators/adaptive_pool.hpp>
#include <boost/interprocess/managed_shared_memory.hpp>
#include <scoped_allocator>
#include <vector>

namespace bi = boost::interprocess;

template<class T> using alloc = bi::adaptive_pool<T,
                                    bi::managed_shared_memory::segment_manager>;
using ipc_row = std::vector<int, alloc<int>>;
using ipc_matrix = std::vector<ipc_row, std::scoped_allocator_adaptor<alloc<ipc_row>>>;

int main ()
{
    bi::managed_shared_memory s(bi::create_only, "Demo", 65536);

    // create vector of vectors in shared memory
    ipc_matrix v(s.get_segment_manager());

    // for all these additions, the inner vectors obtain their allocator arguments
    // from the outer vector's scoped_allocator_adaptor
    v.resize(1);
    v[0].push_back(1);
    v.emplace_back(2);
    std::vector<int> local_row = {1, 2, 3};
    v.emplace_back(local_row.begin(), local_row.end());

    bi::shared_memory_object::remove("Demo");
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2108 | C++11 | there was no way to show if `scoped_allocator_adaptor` is
      stateless | `is_always_equal` provided

### See also

- **allocator_traits (C++11)** — provides information about allocator types
  (class template)
- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)
- **allocator** — the default allocator (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor*
