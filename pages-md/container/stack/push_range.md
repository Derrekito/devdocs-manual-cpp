# std::stack<T,Container>::push_range

```cpp
template< container-compatible-range<value_type> R >
void push_range( R&& rg );  // (since C++23)
```

Inserts a copy of each element of `rg` in `stack`, as if by:

- `c.append_range(std::forward<R>(rg))` if that is a valid expression (i.e. the
  underlying container `c` has an appropriate `append_range` member function),
  or
- `ranges::copy(rg, std::back_inserter(c))` otherwise.

Each iterator in the range `rg` is dereferenced exactly once.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

### Return value

(none)

### Complexity

Identical to the complexity of `c.append_range` or `ranges::copy(rg,
std::back_inserter(c))` (depending on what function is used internally).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <stack>

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
    std::stack<int> adaptor;
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
1 3 2 4
```

### See also

- **push** — inserts element at the top (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/push_range*
