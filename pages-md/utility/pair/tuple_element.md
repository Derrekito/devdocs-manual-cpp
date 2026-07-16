# std::tuple_element<std::pair>

```cpp
template< std::size_t I, class T1, class T2 >
struct tuple_element<I, std::pair<T1, T2>>;  // (since C++11)
```

The partial specializations of `std::tuple_element` for pairs provide
compile-time access to the types of the pair's elements, using tuple-like
syntax. The program is ill-formed if `I >= 2`.

### Member types

- **`type`** — `T1` if `I == 0` `T2` if `I == 1`

### Possible implementation

```cpp
template<std::size_t I, typename T>
struct tuple_element;

template<std::size_t I, typename T1, typename T2>
struct tuple_element<I, std::pair<T1, T2>>
{
    static_assert(I < 2, "std::pair has only 2 elements!");
};

template<typename T1, typename T2>
struct tuple_element<0, std::pair<T1, T2>>
{
    using type = T1;
};

template<typename T1, typename T2>
struct tuple_element<1, std::pair<T1, T2>>
{
    using type = T2;
};
```

### Example

```cpp
#include <iostream>
#include <string>
#include <tuple>

namespace detail
{
    template<std::size_t>
    struct index_tag { constexpr explicit index_tag() = default; };

    template<class T, class U>
    constexpr T get_val_dispatch(std::pair<T, U> const& pair, index_tag<0>)
    {
        return pair.first;
    }

    template<class T, class U>
    constexpr U get_val_dispatch(std::pair<T, U> const& pair, index_tag<1>)
    {
        return pair.second;
    }
} // namespace detail

template<std::size_t N, class T, class U>
auto constexpr get_val(std::pair<T, U> const& pair)
    -> typename std::tuple_element<N, std::pair<T, U>>::type
{
    return detail::get_val_dispatch(pair, detail::index_tag<N>{});
}

int main()
{
    auto var = std::make_pair(1, std::string{"one"});

    std::cout << get_val<0>(var) << " = " << get_val<1>(var);
}
```

Output:

```text
1 = one
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2974 | C++11 | out-of-bounds index referred the undefined primary template
      | made ill-formed (hard error)

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)
- **std::tuple_element<std::array> (C++11)** — obtains the type of the elements
  of `array` (class template specialization)
- **std::tuple_element<std::ranges::subrange> (C++20)** — obtains the type of
  the iterator or the sentinel of a `std::ranges::subrange` (class template
  specialization)
- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/tuple_element*
