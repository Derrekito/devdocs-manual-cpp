# std::ranges::subrange

```cpp
template<
    std::input_or_output_iterator I,
    std::sentinel_for<I> S = I,
    ranges::subrange_kind K = std::sized_sentinel_for<S, I> ?
        ranges::subrange_kind::sized : ranges::subrange_kind::unsized
>
    requires (K == ranges::subrange_kind::sized || !std::sized_sentinel_for<S, I>)
class subrange : public ranges::view_interface<subrange<I, S, K>>  // (since C++20)
```

The `subrange` class template combines together an iterator and a sentinel into
a single `view`.

Additionally, the subrange is a `sized_range` whenever the final template
parameter is `subrange_kind​::​sized` (which happens when
`std::sized_sentinel_for<S, I>` is satisfied or when size is passed explicitly
as a constructor argument). The size record is needed to be stored if and only
if `std::sized_sentinel_for<S, I>` is `false` and `K` is `subrange_kind::sized`.

### Member functions

- **(constructor) (C++20)** — creates a new `subrange` (public member function)
- **operator PairLike (C++20)** — converts the `subrange` to a `pair-like` type
  (public member function)

**Observers**

- **begin (C++20)** — obtains the iterator (public member function)
- **end (C++20)** — obtains the sentinel (public member function)
- **empty (C++20)** — checks whether the `subrange` is empty (public member
  function)
- **size (C++20)** — obtains the size of the `subrange` (public member function)

**Iterator operations**

- **advance (C++20)** — advances the iterator by given distance (public member
  function)
- **prev (C++20)** — obtains a copy of the `subrange` with its iterator
  decremented by a given distance (public member function)
- **next (C++20)** — obtains a copy of the `subrange` with its iterator advanced
  by a given distance (public member function)

**Inherited from `std::ranges::view_interface`**

- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **data (C++20)** — gets the address of derived view's data. Provided if its
  iterator type satisfies `contiguous_iterator`. (public member function of
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

### Non-member functions

- **get(std::ranges::subrange) (C++20)** — obtains iterator or sentinel from a
  `std::ranges::subrange` (function template)

### Helper types

- **ranges::subrange_kind (C++20)** — specifies whether a
  `std::ranges::subrange` models `std::ranges::sized_range` (enum)
- **std::tuple_size<std::ranges::subrange> (C++20)** — obtains the number of
  components of a `std::ranges::subrange` (class template specialization)
- **std::tuple_element<std::ranges::subrange> (C++20)** — obtains the type of
  the iterator or the sentinel of a `std::ranges::subrange` (class template
  specialization)

### Helper templates

```cpp
template< class I, class S, ranges::subrange_kind K >
inline constexpr bool enable_borrowed_range<ranges::subrange<I, S, K>> = true;
```

This specialization of `std::ranges::enable_borrowed_range` makes `subrange`
satisfy `borrowed_range`.

### Example

```cpp
#include <iostream>
#include <map>
#include <ranges>
#include <string_view>

template<class V>
void mutate(V& v)
{
    v += 'A' - 'a';
}

template<class K, class V>
void mutate_map_values(std::multimap<K, V>& m, K k)
{
    auto [first, last] = m.equal_range(k);
    for (auto& [_, v] : std::ranges::subrange(first, last))
        mutate(v);
}

int main()
{
    auto print = [](std::string_view rem, auto const& mm)
    {
        std::cout << rem << "{ ";
        for (const auto& [k, v] : mm)
            std::cout << '{' << k << ",'" << v << "'} ";
        std::cout << "}\n";
    };

    std::multimap<int, char> mm{{4,'a'}, {3,'-'}, {4,'b'}, {5,'-'}, {4,'c'}};
    print("Before: ", mm);
    mutate_map_values(mm, 4);
    print("After:  ", mm);
}
```

Output:

```text
Before: { {3,'-'} {4,'a'} {4,'b'} {4,'c'} {5,'-'} }
After:  { {3,'-'} {4,'A'} {4,'B'} {4,'C'} {5,'-'} }
```

### See also

- **ranges::view_interface (C++20)** — helper class template for defining a
  `view`, using the curiously recurring template pattern (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange*
