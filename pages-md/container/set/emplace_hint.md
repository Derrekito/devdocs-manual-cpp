# std::set<Key,Compare,Allocator>::emplace_hint

```cpp
template< class... Args >
iterator emplace_hint( const_iterator hint, Args&&... args );  // (since C++11)
```

Inserts a new element into the container as close as possible to the position
just before `hint`. The element is constructed in-place, i.e. no copy or move
operations are performed.

The constructor of the element is called with exactly the same arguments as
supplied to the function, forwarded with `std::forward<Args>(args)...`.

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
#include <set>

const int n_operations = 100'500'0;

std::size_t set_emplace()
{
    std::set<int> set;
    for (int i = 0; i < n_operations; ++i)
        set.emplace(i);
    return set.size();
}

std::size_t set_emplace_hint()
{
    std::set<int> set;
    auto it = set.begin();
    for (int i = 0; i < n_operations; ++i)
    {
        set.emplace_hint(it, i);
        it = set.end();
    }
    return set.size();
}

std::size_t set_emplace_hint_wrong()
{
    std::set<int> set;
    auto it = set.begin();
    for (int i = n_operations; i > 0; --i)
    {
        set.emplace_hint(it, i);
        it = set.end();
    }
    return set.size();
}

std::size_t set_emplace_hint_corrected()
{
    std::set<int> set;
    auto it = set.begin();
    for (int i = n_operations; i > 0; --i)
    {
        set.emplace_hint(it, i);
        it = set.begin();
    }
    return set.size();
}

std::size_t set_emplace_hint_closest()
{
    std::set<int> set;
    auto it = set.begin();
    for (int i = 0; i < n_operations; ++i)
        it = set.emplace_hint(it, i);
    return set.size();
}

double time_it(std::function<std::size_t()> set_test,
               const char* what = nullptr,
               double ratio = 0.0)
{
    const auto start = std::chrono::system_clock::now();
    const std::size_t setsize = set_test();
    const auto stop = std::chrono::system_clock::now();
    const std::chrono::duration<double, std::milli> time = stop - start;
    if (what != nullptr && setsize > 0)
        std::cout << std::setw(8) << time << " for " << what << " (ratio: "
                  << (ratio == 0.0 ? 1.0 : ratio / time.count()) << ")\n";
    return time.count();
}

int main()
{
    std::cout << std::fixed << std::setprecision(2);
    time_it(set_emplace); // cache warmup
    const auto x = time_it(set_emplace, "plain emplace");
    time_it(set_emplace_hint, "emplace with correct hint", x);
    time_it(set_emplace_hint_wrong, "emplace with wrong hint", x);
    time_it(set_emplace_hint_corrected, "corrected emplace", x);
    time_it(set_emplace_hint_closest, "emplace using returned iterator", x);
}
```

Possible output:

```text
379.92ms for plain emplace (ratio: 1.00)
 99.36ms for emplace with correct hint (ratio: 3.82)
374.33ms for emplace with wrong hint (ratio: 1.01)
 83.00ms for corrected emplace (ratio: 4.58)
 84.26ms for emplace using returned iterator (ratio: 4.51)
```

### See also

- **emplace (C++11)** — constructs element in-place (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/emplace_hint*
