# std::make_from_tuple

```cpp
template< class T, class Tuple >
constexpr T make_from_tuple( Tuple&& t );  // (since C++17) (until C++23)
template< class T, tuple-like Tuple >
constexpr T make_from_tuple( Tuple&& t );  // (since C++23)
```

Construct an object of type `T`, using the elements of the tuple `t` as the
arguments to the constructor.

Given the exposition-only function `/*make-from-tuple-impl*/` defined as
follows:
`template<class T, tuple-like Tuple, std::size_t... I> // no constraint on Tuple
before C++23 constexpr T /*make-from-tuple-impl*/(Tuple&& t,
std::index_sequence<I...>) { return T(std::get<I>(std::forward<Tuple>(t))...);
}`

The effect is equivalent to:
`return /*make-from-tuple-impl*/<T>( std::forward<Tuple>(t),
std::make_index_sequence<std::tuple_size_v<std::remove_reference_t<Tuple>>>{});`.

If

- `std::tuple_size_v<std::remove_reference_t<Tuple>>` is `1` and
  `std::reference_constructs_from_temporary_v< T,
  decltype(std::get<0>(std::declval<Tuple>()))>` is `true`, or
*(since C++23)*

- `std::is_constructible_v<T, decltype(std::get<I>(std::declval<Tuple>()))...>`
  is `false`,

the program is ill-formed.

### Parameters

- **t** — tuple whose elements to be used as arguments to the constructor of `T`

### Return value

The constructed `T` object or reference.

### Notes

`Tuple` need not be `std::tuple`, and instead may be anything that supports
`std::get` and `std::tuple_size`; in particular, `std::array` and `std::pair`
may be used.
*(until C++23)*

`Tuple` is constrained to be tuple-like, i.e. each type therein is required to
be a specialization of `std::tuple` or another type (such as `std::array` and
`std::pair`) that models `tuple-like`.
*(since C++23)*

Due to guaranteed copy elision, `T` need not be movable.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_make_from_tuple` | 201606L | (C++17) | `std::make_from_tuple`

### Example

```cpp
#include <iostream>
#include <tuple>

struct Foo
{
    Foo(int first, float second, int third)
    {
        std::cout << first << ", " << second << ", " << third << '\n';
    }
};

int main()
{
    auto tuple = std::make_tuple(42, 3.14f, 0);
    std::make_from_tuple<Foo>(std::move(tuple));
}
```

Output:

```text
42, 3.14, 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3528 | C++17 | cast containing reinterpret_cast etc. was allowed in the
      case of 1-tuple | prohibited

### See also

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/make_from_tuple*
