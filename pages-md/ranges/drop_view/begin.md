# std::ranges::drop_view<V>::begin

```cpp
constexpr auto begin()
    requires (!(__SimpleView<V> &&
                 ranges::random_access_range<const V> &&
                 ranges::sized_range<const V>));  // (1) (since C++20)
constexpr auto begin() const
    requires ranges::random_access_range<const V> &&
             ranges::sized_range<const V>;  // (2) (since C++20)
```

Returns an iterator to the first element of the `drop_view`, that is, an
iterator to the *N**th* element of the underlying view, or to the end of the
underlying view if it has less than *N* elements.

If `V` is not a `random_access_range` or a `sized_range`, in order to provide
the amortized constant time complexity required by the `range` concept, the
overload (1) caches the result within the `drop_view` object for use on
subsequent calls.

### Parameters

(none)

### Return value

`ranges::next(ranges::begin(base_), count_, ranges::end(base_))`, where `base_`
is the underlying view, and `count_` is the number of elements to skip.

### Example

```cpp
#include <array>
#include <concepts>
#include <iostream>
#include <iterator>
#include <ranges>

void println(std::ranges::range auto const& range)
{
    for (auto const& elem : range)
        std::cout << elem;
    std::cout << '\n';
}

int main()
{
    std::array hi{'H', 'e', 'l', 'l', 'o', ',', ' ', 'C', '+', '+', '2', '0', '!'};
    println(hi);

    const auto pos = std::distance(hi.begin(), std::ranges::find(hi, 'C'));
    auto cxx = std::ranges::drop_view{hi, pos};
    std::cout << "*drop_view::begin() == '" << *cxx.begin() << "'\n";
//  *cxx.begin() = 'c'; // undefined: 'views' are to be used as observers
    println(cxx);
}
```

Output:

```text
Hello, C++20!
*drop_view::begin() == 'C'
C++20!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3482 | C++20 | the const overload can be called with unsized ranges | the
      const overload requires `sized_range`

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_view/begin*
