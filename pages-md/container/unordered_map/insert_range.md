# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::insert_range

```cpp
template< container-compatible-range<value_type> R >
void insert_range( R&& rg );  // (since C++23)
```

Inserts a copy of each element of `rg` if and only if there is no element with
key equivalent to the key of that element in `*this`.

Each iterator in the range `rg` is dereferenced exactly once. The behavior is
undefined if `rg` overlaps with the container.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`value_type` must be EmplaceConstructible into `unordered_map` from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

Average case O(N), where N is `ranges::distance(rg)`, worst case O(`N·(a.size()
+ 1)`).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <iostream>
#include <unordered_map>
#include <utility>

void println(auto, auto const& container)
{
    for (const auto& [key, value] : container)
        std::cout << '{' << key << ',' << value << '}' << ' ';
    std::cout << '\n';
}

int main()
{
    auto container = std::unordered_map{std::pair{1, 11}, {3, 33}, {2, 22}, {4, 44}};
    const auto rg = {std::pair{-1, -11}, {3, -33}, {-2, -22}};

#ifdef __cpp_lib_containers_ranges
    container.insert_range(rg);
#else
    container.insert(rg.begin(), rg.end());
#endif

    println("{}", container);
}
```

Possible output:

```text
{4,44} {-1,-11} {2,22} {3,33} {-2,-22} {1,11}
```

### See also

- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/insert_range*
