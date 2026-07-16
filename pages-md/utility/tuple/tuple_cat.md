# std::tuple_cat

```cpp
template< class... Tuples >
std::tuple</* CTypes */...> tuple_cat( Tuples&&... args );  // (since C++11) (until C++14)
template< class... Tuples >
constexpr std::tuple</* CTypes */...> tuple_cat( Tuples&&... args );  // (since C++14) (until C++23)
template< tuple-like... Tuples >
constexpr std::tuple</* CTypes */...> tuple_cat( Tuples&&... args );  // (since C++23)
```

Constructs a tuple that is a concatenation of all tuples in `args`. The element
types /* CTypes */ of the returned tuple is formed by concatenating the elements
type packs of all `std::tuple`(until C++23)`tuple-like`(since C++23) types in
`Tuples` in order.

The behavior is undefined if any type in std::decay_t<Tuples>... is not a
specialization of `std::tuple`. However, an implementation may choose to support
types (such as `std::array` and `std::pair`) that follow the tuple-like
protocol.
*(until C++23)*

The types std::decay_t<Tuples>... are constrained to be tuple-like, i.e. each
type therein is required to be a specialization of `std::tuple` or another type
(such as `std::array` and `std::pair`) that models `tuple-like`.
*(since C++23)*

If any type in /* CTypes */ is not constructible from the type of the
corresponding element in the sequence of elements concatenated from `args`, the
behavior is undefined(until C++23)the program is ill-formed(since C++23).

### Parameters

- **args** — zero or more tuples to concatenate

### Return value

A `std::tuple` object composed of all elements of all argument tuples
constructed from `std::get<j>(std::forward<Ti>(arg))` for each individual
element.

### Example

```cpp
#include <iostream>
#include <string>
#include <tuple>

// helper function to print a tuple of any size
template<class Tuple, std::size_t N>
struct TuplePrinter
{
    static void print(const Tuple& t)
    {
        TuplePrinter<Tuple, N - 1>::print(t);
        std::cout << ", " << std::get<N-1>(t);
    }
};

template<class Tuple>
struct TuplePrinter<Tuple, 1>
{
    static void print(const Tuple& t)
    {
        std::cout << std::get<0>(t);
    }
};

template<typename... Args, std::enable_if_t<sizeof...(Args) == 0, int> = 0>
void print(const std::tuple<Args...>& t)
{
    std::cout << "()\n";
}

template<typename... Args, std::enable_if_t<sizeof...(Args) != 0, int> = 0>
void print(const std::tuple<Args...>& t)
{
    std::cout << "(";
    TuplePrinter<decltype(t), sizeof...(Args)>::print(t);
    std::cout << ")\n";
}
// end helper function

int main()
{
    std::tuple<int, std::string, float> t1(10, "Test", 3.14);
    int n = 7;
    auto t2 = std::tuple_cat(t1, std::make_tuple("Foo", "bar"), t1, std::tie(n));
    n = 42;
    print(t2);
}
```

Output:

```text
(10, Test, 3.14, Foo, bar, 10, Test, 3.14, 42)
```

### See also

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tuple_cat*
