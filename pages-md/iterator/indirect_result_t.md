# std::indirect_result_t

```cpp
template< class F, class... Is >
    requires (std::indirectly_readable<Is> && ...) &&
              std::invocable<F, std::iter_reference_t<Is>...>
using indirect_result_t = std::invoke_result_t<F, std::iter_reference_t<Is>...>;  // (since C++20)
```

The alias template `indirect_result_t` obtains the result type of invoking an
`invocable` type `F` on the result of dereferencing `indirectly_readable` types
`Is...`.

### Template parameters

- **F** — an invocable type
- **Is** — indirectly readable types that are dereferenced to arguments

### Example

```cpp
#include <iterator>
#include <type_traits>

struct Fn
{
    long operator()(const int&);
    int operator()(int&&);
    short operator()(int, int) const;
    auto operator()(const float) -> int&;
    void operator()(int[8]);
};

static_assert(
    std::is_same_v<std::indirect_result_t<Fn, const int*>, long> and
    std::is_same_v<std::indirect_result_t<Fn, std::move_iterator<int*>>, int> and
    std::is_same_v<std::indirect_result_t<const Fn, int*, int*>, short> and
    std::is_same_v<std::indirect_result_t<Fn, float*>, int&> and
    std::is_same_v<std::indirect_result_t<Fn, int**>, void>
);

int main() {}
```

### See also

- **result_ofinvoke_result (C++11)(removed in C++20)(C++17)** — deduces the
  result type of invoking a callable object with a set of arguments (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirect_result_t*
