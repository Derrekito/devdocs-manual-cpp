# std::weak_ptr

```cpp
template< class T > class weak_ptr;  // (since C++11)
```

`std::weak_ptr` is a smart pointer that holds a non-owning ("weak") reference to
an object that is managed by `std::shared_ptr`. It must be converted to
`std::shared_ptr` in order to access the referenced object.

`std::weak_ptr` models temporary ownership: when an object needs to be accessed
only if it exists, and it may be deleted at any time by someone else,
`std::weak_ptr` is used to track the object, and it is converted to
`std::shared_ptr` to assume temporary ownership. If the original
`std::shared_ptr` is destroyed at this time, the object's lifetime is extended
until the temporary `std::shared_ptr` is destroyed as well.

Another use for `std::weak_ptr` is to break reference cycles formed by objects
managed by `std::shared_ptr`. If such cycle is orphaned (i.e., there are no
outside shared pointers into the cycle), the `shared_ptr` reference counts
cannot reach zero and the memory is leaked. To prevent this, one of the pointers
in the cycle can be made weak.

### Member types

- **`element_type`** — `T` (until C++17) `std::remove_extent_t<T>` (since C++17)
- **`T`** — (until C++17)
- **`std::remove_extent_t<T>`** — (since C++17)

### Member functions

- **(constructor)** — creates a new `weak_ptr` (public member function)
- **(destructor)** — destroys a `weak_ptr` (public member function)
- **operator=** — assigns the `weak_ptr` (public member function)

**Modifiers**

- **reset** — releases the ownership of the managed object (public member
  function)
- **swap** — swaps the managed objects (public member function)

**Observers**

- **use_count** — returns the number of `shared_ptr` objects that manage the
  object (public member function)
- **expired** — checks whether the referenced object was already deleted (public
  member function)
- **lock** — creates a `shared_ptr` that manages the referenced object (public
  member function)
- **owner_before** — provides owner-based ordering of weak pointers (public
  member function)
- **owner_hash (C++26)** — provides owner-based hashing of weak pointers (public
  member function)
- **owner_equal (C++26)** — provides owner-based equal comparison of weak
  pointers (public member function)

### Non-member functions

- **std::swap(std::weak_ptr) (C++11)** — specializes the `std::swap` algorithm
  (function template)

### Helper classes

- **std::atomic<std::weak_ptr> (C++20)** — atomic weak pointer (class template
  specialization)

### Deduction guides (since C++17)

### Notes

Like `std::shared_ptr`, a typical implementation of `weak_ptr` stores two
pointers:

- a pointer to the control block; and
- the stored pointer of the `shared_ptr` it was constructed from.

A separate stored pointer is necessary to ensure that converting a `shared_ptr`
to `weak_ptr` and then back works correctly, even for aliased `shared_ptr`s. It
is not possible to access the stored pointer in a `weak_ptr` without locking it
into a `shared_ptr`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_smart_ptr_owner_equality` | 202306L | (C++26) | Enabling the use of
      `std::weak_ptr` as keys in unordered associative containers

### Example

Demonstrates how lock is used to ensure validity of the pointer.

```cpp
#include <iostream>
#include <memory>

std::weak_ptr<int> gw;

void observe()
{
    std::cout << "gw.use_count() == " << gw.use_count() << "; ";
    // we have to make a copy of shared pointer before usage:
    if (std::shared_ptr<int> spt = gw.lock())
        std::cout << "*spt == " << *spt << '\n';
    else
        std::cout << "gw is expired\n";
}

int main()
{
    {
        auto sp = std::make_shared<int>(42);
        gw = sp;

        observe();
    }

    observe();
}
```

Output:

```text
gw.use_count() == 1; *spt == 42
gw.use_count() == 0; gw is expired
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3001 | C++17 | `element_type` was not updated for array support | updated

### See also

- **unique_ptr (C++11)** — smart pointer with unique object ownership semantics
  (class template)
- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr*
