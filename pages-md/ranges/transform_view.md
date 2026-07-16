# std::ranges::views::transform, std::ranges::transform_view

```cpp
template< ranges::input_range V,
          std::copy_constructible F >
    requires ranges::view<V> &&
             std::is_object_v<F> &&
             std::regular_invocable<F&, ranges::range_reference_t<V>> &&
             /* invoke_result_t<F&, range_reference_t<V>>& is a valid type */
class transform_view
    : public ranges::view_interface<transform_view<V, F>>  // (1) (since C++20)
namespace views {
    inline constexpr /*unspecified*/ transform = /*unspecified*/;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class F >
    requires /* see below */
constexpr ranges::view auto transform( R&& r, F&& fun );  // (since C++20)
template< class F >
constexpr /*range adaptor closure*/ transform( F&& fun );  // (since C++20)
```

1) A range adaptor that represents `view` of an underlying sequence after
   applying a transformation function to each element.

2) RangeAdaptorObject. The expression `views::transform(e, f)` is
   expression-equivalent to `transform_view(e, f)` for any suitable
   subexpressions `e` and `f`.

`transform_view` models the concepts `random_access_range`,
`bidirectional_range`, `forward_range`, `input_range`, `common_range`, and
`sized_range` when the underlying view `V` models respective concepts.

### Member functions

- **(constructor) (C++20)** — constructs a `transform_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++20)** — returns the number of elements. Provided only if the
  underlying (adapted) range satisfies `sized_range`. (public member function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Nested classes

- ***iterator* (C++20)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++20)** — the sentinel type (exposition-only member class
  template*)

### Example

```cpp
#include <algorithm>
#include <cstdio>
#include <iterator>
#include <ranges>
#include <string>

char rot13a(const char x, const char a)
{
    return a + (x - a + 13) % 26;
}

char rot13(const char x)
{
    if ('Z' >= x and x >= 'A')
        return rot13a(x, 'A');

    if ('z' >= x and x >= 'a')
        return rot13a(x, 'a');

    return x;
}

int main()
{
    auto show = [](const unsigned char x) { std::putchar(x); };

    std::string in{"cppreference.com\n"};
    std::ranges::for_each(in, show);
    std::ranges::for_each(in | std::views::transform(rot13), show);

    std::string out;
    std::ranges::copy(std::views::transform(in, rot13), std::back_inserter(out));
    std::ranges::for_each(out, show);
    std::ranges::for_each(out | std::views::transform(rot13), show);
}
```

Output:

```text
cppreference.com
pccersrerapr.pbz
pccersrerapr.pbz
cppreference.com
```

### See also

- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view*
