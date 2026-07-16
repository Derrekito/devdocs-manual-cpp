# std::raw_storage_iterator

```cpp
template< class OutputIt, class T >
class raw_storage_iterator
    : public std::iterator<std::output_iterator_tag, void, void, void, void>;  // (until C++17)
template< class OutputIt, class T >
class raw_storage_iterator;  // (since C++17) (deprecated in C++17) (removed in C++20)
```

The output iterator `std::raw_storage_iterator` makes it possible for standard
algorithms to store results in uninitialized memory. Whenever the algorithm
writes an object of type `T` to the dereferenced iterator, the object is
copy-constructed into the location in the uninitialized storage pointed to by
the iterator. The template parameter `OutputIt` is any type that meets the
requirements of LegacyOutputIterator and has `operator*` defined to return an
object, for which `operator&` returns an object of type `T*`. Usually, the type
`T*` is used as `OutputIt`.

### Type requirements

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

### Member functions

- **(constructor)** — creates a new `raw_storage_iterator` (public member
  function)
- **operator=** — constructs an object at the pointed-to location in the buffer
  (public member function)
- **operator*** — dereferences the iterator (public member function)
- **operator++operator++(int)** — advances the iterator (public member function)
- **base (since C++17)** — provides access to the wrapped iterator (public
  member function)

### Member types

- **`iterator_category`** — std::output_iterator_tag
- **`value_type`** — void
- **`difference_type`** — void (until C++20) `std::ptrdiff_t` (since C++20)
- **void** — (until C++20)
- **`std::ptrdiff_t`** — (since C++20)
- **`pointer`** — void
- **`reference`** — void

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from
std::iterator<std::output_iterator_tag, void, void, void, void>.
*(until C++17)*

### Note

`std::raw_storage_iterator` was deprecated primarily because of its
exception-unsafe behavior. Unlike `std::uninitialized_copy`, it doesn't handle
exceptions during operations like `std::copy` safely, potentially leading to
resource leaks due to a lack of tracking the number of successfully constructed
objects and their proper destruction in the presence of exceptions.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    const std::string s[] = {"This", "is", "a", "test", "."};
    std::string* p = std::allocator<std::string>().allocate(5);

    std::copy(std::begin(s), std::end(s),
              std::raw_storage_iterator<std::string*, std::string>(p));

    for (std::string* i = p; i != p + 5; ++i)
    {
        std::cout << *i << '\n';
        i->~basic_string<char>();
    }
    std::allocator<std::string>().deallocate(p, 5);
}
```

Output:

```text
This
is
a
test
.
```

### See also

- **allocator_traits (C++11)** — provides information about allocator types
  (class template)
- **scoped_allocator_adaptor (C++11)** — implements multi-level allocator for
  multi-level containers (class template)
- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/raw_storage_iterator*
