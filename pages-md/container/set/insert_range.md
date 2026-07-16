# std::set<Key,Compare,Allocator>::insert_range

```cpp
template< container-compatible-range<value_type> R >
void insert_range( R&& rg );  // (since C++23)
```

Inserts a copy of each element of `rg` if and only if there is no element with
key equivalent to the key of that element in `*this`.

Each iterator in the range `rg` is dereferenced exactly once. The behavior is
undefined if `rg` overlaps with the container.

No iterators or references are invalidated.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`value_type` must be EmplaceConstructible into `set` from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

`N·log(a.size() + N)`, where `N` has the value `ranges::distance(rg)`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <iostream>
#include <set>

void println(auto, auto const& container)
{
    for (const auto& elem : container)
        std::cout << elem << ' ';
    std::cout << '\n';
}

int main()
{
    auto container = std::set{1, 3, 2, 4};
    const auto rg = {-1, 3, -2};

#ifdef __cpp_lib_containers_ranges
    container.insert_range(rg);
#else
    container.insert(rg.begin(), rg.end());
#endif

    println("{}", container);
}
```

Output:

```text
-2 -1 1 2 3 4
```

### See also

- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/insert_range*
