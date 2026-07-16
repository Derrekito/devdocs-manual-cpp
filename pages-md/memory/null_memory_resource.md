# std::pmr::null_memory_resource

```cpp
std::pmr::memory_resource* null_memory_resource() noexcept;  // (since C++17)
```

Returns a pointer to a `memory_resource` that doesn't perform any allocation.

### Return value

Returns a pointer `p` to a static storage duration object of a type derived from
`std::pmr::memory_resource`, with the following properties:

- its `allocate()` function always throws `std::bad_alloc`;
- its `deallocate()` function has no effect;
- for any `memory_resource` `r`, `p->is_equal(r)` returns `&r == p`.

The same value is returned every time this function is called.

### Example

The program demos the main usage of `null_memory_resource`: ensure that a memory
pool which requires memory allocated on the stack will NOT allocate memory on
the heap if it needs more memory.

```cpp
#include <array>
#include <cstddef>
#include <iostream>
#include <memory_resource>
#include <string>
#include <unordered_map>

int main()
{
    // allocate memory on the stack
    std::array<std::byte, 20000> buf;

    // without fallback memory allocation on heap
    std::pmr::monotonic_buffer_resource pool{buf.data(), buf.size(),
                                             std::pmr::null_memory_resource()};

    // allocate too much memory
    std::pmr::unordered_map<long, std::pmr::string> coll{&pool};
    try
    {
        for (std::size_t i = 0; i < buf.size(); ++i)
        {
            coll.emplace(i, "just a string with number " + std::to_string(i));

            if (i && i % 50 == 0)
                std::clog << "size: " << i << "...\n";
        }
    }
    catch (const std::bad_alloc& e)
    {
        std::cerr << e.what() << '\n';
    }

    std::cout << "size: " << coll.size() << '\n';
}
```

Possible output:

```text
size: 50...
size: 100...
size: 150...
std::bad_alloc
size: 183
```

---
*Source: https://en.cppreference.com/w/cpp/memory/null_memory_resource*
