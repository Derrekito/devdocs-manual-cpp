# std::apply

```cpp
template< class F, class Tuple >
constexpr decltype(auto) apply( F&& f, Tuple&& t );  // (since C++17) (until C++23)
template< class F, tuple-like Tuple >
constexpr decltype(auto) apply( F&& f, Tuple&& t ) noexcept(/* see below */);  // (since C++23)
```

Invoke the Callable object `f` with the elements of `t` as arguments.

Given the exposition-only function `apply-impl` defined as follows:
`template<class F, tuple-like Tuple, std::size_t... I> // no constraint on Tuple
before C++23 constexpr decltype(auto) apply-impl(F&& f, Tuple&& t,
std::index_sequence<I...>) // exposition only { return
INVOKE(std::forward<F>(f), std::get<I>(std::forward<Tuple>(t))...); }`

The effect is equivalent to `return apply-impl(std::forward<F>(f),
std::forward<Tuple>(t), std::make_index_sequence<
std::tuple_size_v<std::decay_t<Tuple>>>{});`.

### Parameters

- **f** — Callable object to be invoked
- **t** — tuple whose elements to be used as arguments to `f`

### Return value

The value returned by `f`.

### Exceptions

(none)
*(until C++23)*

`noexcept` specification:
`noexcept( noexcept(std::invoke(std::forward<F>(f),
std::get<Is>(std::forward<Tuple>(t))...)) )`
where `Is...` denotes the parameter pack:
- `0, 1, ..., std::tuple_size_v<std::remove_reference_t<Tuple>> - 1`.
*(since C++23)*

### Notes

`Tuple` need not be `std::tuple`, and instead may be anything that supports
`std::get` and `std::tuple_size`; in particular, `std::array` and `std::pair`
may be used.
*(until C++23)*

`Tuple` is constrained to be tuple-like, i.e. each type therein is required to
be a specialization of `std::tuple` or another type (such as `std::array` and
`std::pair`) that models `tuple-like`.
*(since C++23)*

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_apply` | 201603L | (C++17) | `std::apply`

### Example

```cpp
#include <iostream>
#include <tuple>
#include <utility>

int add(int first, int second) { return first + second; }

template<typename T>
T add_generic(T first, T second) { return first + second; }

auto add_lambda = [](auto first, auto second) { return first + second; };

template<typename... Ts>
std::ostream& operator<<(std::ostream& os, std::tuple<Ts...> const& theTuple)
{
    std::apply
    (
        [&os](Ts const&... tupleArgs)
        {
            os << '[';
            std::size_t n{0};
            ((os << tupleArgs << (++n != sizeof...(Ts) ? ", " : "")), ...);
            os << ']';
        }, theTuple
    );
    return os;
}

int main()
{
    // OK
    std::cout << std::apply(add, std::pair(1, 2)) << '\n';

    // Error: can't deduce the function type
    // std::cout << std::apply(add_generic, std::make_pair(2.0f, 3.0f)) << '\n';

    // OK
    std::cout << std::apply(add_lambda, std::pair(2.0f, 3.0f)) << '\n';

    // advanced example
    std::tuple myTuple{25, "Hello", 9.31f, 'c'};
    std::cout << myTuple << '\n';
}
```

Output:

```text
3
5
[25, Hello, 9.31, c]
```

### See also

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **make_from_tuple (C++17)** — construct an object with a tuple of arguments
  (function template)
- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/apply*
