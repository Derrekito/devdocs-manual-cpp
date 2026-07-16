# std::priority_queue<T,Container,Compare>::push_range

```cpp
template< container-compatible-range<value_type> R >
void push_range( R&& rg );  // (since C++23)
```

Inserts a copy of each element of `rg` in `priority_queue`, as if by:

- `c.append_range(std::forward<R>(rg))` if that is a valid expression (i.e. the
  underlying container `c` has an appropriate `append_range` member function),
  or
- `ranges::copy(rg, std::back_inserter(c))` otherwise.

Then restores the heap property as if by `ranges::make_heap(c, comp)`. After the
insertion `ranges::is_heap(c, comp)` is true.

Each iterator in the range `rg` is dereferenced exactly once.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

### Return value

(none)

### Complexity

The complexity of `c.append_range` plus the complexity of `ranges::make_heap(c,
comp)`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <queue>

template<typename Adaptor>
requires (std::ranges::input_range<typename Adaptor::container_type>)
void println(auto, const Adaptor& adaptor)
{
    struct Container : Adaptor // gain access to protected Adaptor::Container c;
    {
        auto const& container() const { return this->c; }
    };

    for (auto const& elem : static_cast<const Container&>(adaptor).container())
        std::cout << elem << ' ';
    std::cout << '\n';
}

int main()
{
    std::priority_queue<int> adaptor;
    const auto rg = {1, 3, 2, 4};

#ifdef __cpp_lib_containers_ranges
    adaptor.push_range(rg);
#else
    std::ranges::for_each(rg, [&adaptor](auto e){ adaptor.push(e); });
#endif

    println("{}", adaptor);
}
```

Output:

```text
4 3 2 1
```

### See also

- **push** — inserts element and sorts the underlying container (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/push_range*
