# std::hash<std::optional>

```cpp
template< class T >
struct hash<std::optional<T>>;  // (since C++17)
```

The template specialization of `std::hash` for the `std::optional` class allows
users to obtain hashes of the values contained in `optional` objects.

The specialization `std::hash<std::optional<T>>` is enabled (see `std::hash`) if
`std::hash<std::remove_const_t<T>>` is enabled, and is disabled otherwise.

When enabled, for an object `o` of type `std::optional<T>` that contains a
value, `std::hash<std::optional<T>>()(o)` evaluates to the same value as
`std::hash<std::remove_const_t<T>>()(*o)`. For an optional that does not contain
a value, the hash is unspecified.

The member functions of this specialization are not guaranteed to be noexcept
because the hash of the underlying type might throw.

### Template parameters

- **T** — the type of the value contained in `optional` object

### Example

```cpp
#include <iostream>
#include <optional>
#include <string>
#include <unordered_set>

using namespace std::literals;

int main()
{
    using OptStr = std::optional<std::string>;

    // hash<optional> makes it possible to use unordered_set
    std::unordered_set<OptStr> s =
    {
        "ABC"s, "abc"s, std::nullopt, "def"s
    };

    for (const auto& o : s)
        std::cout << o.value_or("(null)") << '\t' << std::hash<OptStr>{}(o) << '\n';
}
```

Possible output:

```text
def     11697390762615875584
(null)  18446744073709548283
abc     3663726644998027833
ABC     11746482041453314842
```

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/hash*
