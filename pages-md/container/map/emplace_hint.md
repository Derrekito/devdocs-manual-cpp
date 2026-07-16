# std::map<Key,T,Compare,Allocator>::emplace_hint

```cpp
template< class... Args >
iterator emplace_hint( const_iterator hint, Args&&... args );  // (since C++11)
```

Inserts a new element to the container as close as possible to the position just
before `hint`. The element is constructed in-place, i.e. no copy or move
operations are performed.

The constructor of the element type (`value_type`, that is, `std::pair<const
Key, T>`) is called with exactly the same arguments as supplied to the function,
forwarded with `std::forward<Args>(args)...`.

No iterators or references are invalidated.

### Parameters

- **hint** — iterator to the position before which the new element will be
  inserted
- **args** — arguments to forward to the constructor of the element

### Return value

Returns an iterator to the newly inserted element.

If the insertion failed because the element already exists, returns an iterator
to the already existing element with the equivalent key.

### Exceptions

If an exception is thrown by any operation, this function has no effect (strong
exception guarantee).

### Complexity

Logarithmic in the size of the container in general, but amortized constant if
the new element is inserted just before `hint`.

### Example

```cpp
#include <chrono>
#include <cstddef>
#include <functional>
#include <iomanip>
#include <iostream>
#include <map>

const int n_operations = 100'500'0;

std::size_t map_emplace()
{
    std::map<int, char> map;
    for (int i = 0; i < n_operations; ++i)
        map.emplace(i, 'a');

    return map.size();
}

std::size_t map_emplace_hint()
{
    std::map<int, char> map;
    auto it = map.begin();
    for (int i = 0; i < n_operations; ++i)
    {
        map.emplace_hint(it, i, 'b');
        it = map.end();
    }
    return map.size();
}

std::size_t map_emplace_hint_wrong()
{
    std::map<int, char> map;
    auto it = map.begin();
    for (int i = n_operations; i > 0; --i)
    {
        map.emplace_hint(it, i, 'c');
        it = map.end();
    }
    return map.size();
}

std::size_t map_emplace_hint_corrected()
{
    std::map<int, char> map;
    auto it = map.begin();
    for (int i = n_operations; i > 0; --i)
    {
        map.emplace_hint(it, i, 'd');
        it = map.begin();
    }
    return map.size();
}

std::size_t map_emplace_hint_closest()
{
    std::map<int, char> map;
    auto it = map.begin();
    for (int i = 0; i < n_operations; ++i)
        it = map.emplace_hint(it, i, 'e');

    return map.size();
}

double time_it(std::function<std::size_t()> map_test,
               std::string what = "", double ratio = 0.0)
{
    auto start = std::chrono::system_clock::now();
    std::size_t mapsize = map_test();
    auto stop = std::chrono::system_clock::now();
    std::chrono::duration<double, std::milli> time = stop - start;
    if (what.size() > 0 && mapsize > 0)
        std::cout << std::setw(8) << time << " for " << what << " (ratio: "
                  << (ratio == 0.0 ? 1.0 : ratio / time.count()) << ")\n";
    return time.count();
}

int main()
{
    std::cout << std::fixed << std::setprecision(2);
    time_it(map_emplace); // cache warmup
    const auto x = time_it(map_emplace, "plain emplace");
    time_it(map_emplace_hint, "emplace with correct hint", x);
    time_it(map_emplace_hint_wrong, "emplace with wrong hint", x);
    time_it(map_emplace_hint_corrected, "corrected emplace", x);
    time_it(map_emplace_hint_closest, "emplace using returned iterator", x);
}
```

Possible output:

```text
347.88ms for plain emplace (ratio: 1.00)
104.98ms for emplace with correct hint (ratio: 3.31)
388.57ms for emplace with wrong hint (ratio: 0.90)
 85.50ms for corrected emplace (ratio: 4.07)
 85.25ms for emplace using returned iterator (ratio: 4.08)
```

### See also

- **emplace (C++11)** — constructs element in-place (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/emplace_hint*
