# std::set<Key,Compare,Allocator>::emplace

```cpp
template< class... Args >
std::pair<iterator, bool> emplace( Args&&... args );  // (since C++11)
```

Inserts a new element into the container constructed in-place with the given
`args` if there is no element with the key in the container.

Careful use of `emplace` allows the new element to be constructed while avoiding
unnecessary copy or move operations. The constructor of the new element is
called with exactly the same arguments as supplied to `emplace`, forwarded via
`std::forward<Args>(args)...`. The element may be constructed even if there
already is an element with the key in the container, in which case the newly
constructed element will be destroyed immediately.

No iterators or references are invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

Returns a pair consisting of an iterator to the inserted element, or the
already-existing element if no insertion happened, and a bool denoting whether
the insertion took place (true if insertion happened, false if it did not).

### Exceptions

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Complexity

Logarithmic in the size of the container.

### Example

```cpp
#include <chrono>
#include <cstddef>
#include <functional>
#include <iomanip>
#include <iostream>
#include <set>
#include <string>

class Dew
{
private:
    int a;
    int b;
    int c;

public:
    Dew(int _a, int _b, int _c)
        : a(_a), b(_b), c(_c)
    {}

    bool operator<(const Dew& other) const
    {
        return (a < other.a) ||
               (a == other.a && b < other.b) ||
               (a == other.a && b == other.b && c < other.c);
    }
};

constexpr int nof_operations{101};

std::size_t set_emplace()
{
    std::set<Dew> set;
    for (int i = 0; i < nof_operations; ++i)
        for (int j = 0; j < nof_operations; ++j)
            for (int k = 0; k < nof_operations; ++k)
                set.emplace(i, j, k);

    return set.size();
}

std::size_t set_insert()
{
    std::set<Dew> set;
    for (int i = 0; i < nof_operations; ++i)
        for (int j = 0; j < nof_operations; ++j)
            for (int k = 0; k < nof_operations; ++k)
                set.insert(Dew(i, j, k));

    return set.size();
}

void time_it(std::function<int()> set_test, std::string what = "")
{
    const auto start = std::chrono::system_clock::now();
    const auto the_size = set_test();
    const auto stop = std::chrono::system_clock::now();
    const std::chrono::duration<double, std::milli> time = stop - start;
    if (!what.empty() && the_size)
        std::cout << std::fixed << std::setprecision(2)
                  << time << " for " << what << '\n';
}

int main()
{
    time_it(set_insert, "cache warming...");
    time_it(set_insert, "insert");
    time_it(set_insert, "insert");
    time_it(set_emplace, "emplace");
    time_it(set_emplace, "emplace");
}
```

Possible output:

```text
630.58ms for cache warming...
577.16ms for insert
560.84ms for insert
547.10ms for emplace
549.44ms for emplace
```

### See also

- **emplace_hint (C++11)** — constructs elements in-place using a hint (public
  member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/emplace*
