# std::ranges::drop_while_view<V,Pred>::pred

```cpp
constexpr const Pred& pred() const;  // (since C++20)
```

Returns a reference to the stored predicate.

If `*this` does not store a predicate (e.g. an exception is thrown on the
assignment to `*this`, which copy-constructs or move-constructs a `Pred`), the
behavior is undefined.

### Parameters

(none)

### Return value

A reference to the stored predicate.

### Example

```cpp
#include <array>
#include <iomanip>
#include <iostream>
#include <ranges>

int main()
{
    constexpr std::array data{0, -1, -2, 3, 1, 4, 1, 5};

    auto view = std::ranges::drop_while_view
    {
        data, [](int x) { return x <= 0; }
    };

    std::cout << std::boolalpha;
    for (int x : data)
        std::cout << "predicate(" << std::setw(2) << x << ") : "
                  << view.pred()(x) << '\n';
}
```

Output:

```text
predicate( 0) : true
predicate(-1) : true
predicate(-2) : true
predicate( 3) : false
predicate( 1) : false
predicate( 4) : false
predicate( 1) : false
predicate( 5) : false
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_while_view/pred*
