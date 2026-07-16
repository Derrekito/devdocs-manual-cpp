# std::ranges::fill

```cpp
Call signature
template< class T, std::output_iterator<const T&> O, std::sentinel_for<O> S >
constexpr O
    fill( O first, S last, const T& value );  // (1) (since C++20)
template< class T, ranges::output_range<const T&> R >
constexpr ranges::borrowed_iterator_t<R>
    fill( R&& r, const T& value );  // (2) (since C++20)
```

1) Assigns the given `value` to the elements in the range
   `[``first``,``last``)`.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to modify
- **r** — the range of elements to modify
- **value** — the value to be assigned

### Return value

An output iterator that compares equal to `last`.

### Complexity

Exactly `last - first` assignments.

### Possible implementation

```cpp
struct fill_fn
{
    template<class T, std::output_iterator<const T&> O, std::sentinel_for<O> S>
    constexpr O operator()(O first, S last, const T& value) const
    {
        while (first != last)
            *first++ = value;

        return first;
    }

    template<class T, ranges::output_range<const T&> R>
    constexpr ranges::borrowed_iterator_t<R> operator()(R&& r, const T& value) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), value);
    }
};

inline constexpr fill_fn fill;
```

### Example

The following code uses `ranges::fill` to set all elements of std::vector<int>
first to `-1`, then to `10`.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

void println(std::vector<int> const& vi)
{
    for (int e : vi)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> v {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    std::ranges::fill(v.begin(), v.end(), -1);
    println(v);

    std::ranges::fill(v, 10);
    println(v);
}
```

Output:

```text
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
10 10 10 10 10 10 10 10 10 10
```

### See also

- **ranges::fill_n (C++20)** — assigns a value to a number of elements
  (niebloid)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::generate (C++20)** — saves the result of a function in a range
  (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **fill** — copy-assigns the given value to every element in a range (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/fill*
