# std::tuple_element<std::tuple>

```cpp
template< std::size_t I, class... Types >
struct tuple_element< I, std::tuple<Types...> >;  // (since C++11)
```

Provides compile-time indexed access to the types of the elements of the tuple.

### Member types

- **type** — the type of `I`th element of the tuple, where `I` is in
  `[``​0​``,``sizeof...(Types)``)`

### Possible implementation

```cpp
template<std::size_t I, class T>
struct tuple_element;

#ifndef __cpp_pack_indexing
// recursive case
template<std::size_t I, class Head, class... Tail>
struct tuple_element<I, std::tuple<Head, Tail...>>
    : std::tuple_element<I - 1, std::tuple<Tail...>>
{ };

// base case
template<class Head, class... Tail>
struct tuple_element<0, std::tuple<Head, Tail...>>
{
    using type = Head;
};

#else
// C++26 implementation using pack indexing
template<std::size_t I, class... Ts>
struct tuple_element<I, std::tuple<Ts...>>
{
    using type = Ts...[I];
};
#endif
```

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <string>
#include <tuple>
#include <typeinfo>
#include <type_traits>
#include <utility>

namespace details
{
    template<class TupleLike, std::size_t I>
    void printTypeAtIndex()
    {
        std::cout << "  The type at index " << I << " is: ";
        using SelectedType = std::tuple_element_t<I, TupleLike>;
        std::cout << typeid(SelectedType).name() << '\n';
    }
}

template<typename TupleLike, std::size_t I = 0>
void printTypes()
{
    if constexpr (I == 0)
        std::cout << typeid(TupleLike).name() << '\n';

    if constexpr (I < std::tuple_size_v<TupleLike>)
    {
        details::printTypeAtIndex<TupleLike, I>();
        printTypes<TupleLike, I + 1>();
    }
}

struct MyStruct {};

using MyTuple = std::tuple<int, long, char, bool, std::string, MyStruct>;

using MyPair = std::pair<char, bool>;

static_assert
(
    std::is_same_v<std::tuple_element_t<0, MyPair>, char> &&
    std::is_same_v<std::tuple_element_t<1, MyPair>, bool>
);

int main()
{
    printTypes<MyTuple>();
    printTypes<MyPair>();
}
```

Possible output:

```text
St5tupleIJilcbNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE8MyStructEE
  The type at index 0 is: i
  The type at index 1 is: l
  The type at index 2 is: c
  The type at index 3 is: b
  The type at index 4 is: NSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE
  The type at index 5 is: 8MyStruct
St4pairIcbE
  The type at index 0 is: c
  The type at index 1 is: b
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tuple_element*
