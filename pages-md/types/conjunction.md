# std::conjunction

```cpp
template< class... B >
struct conjunction;  // (since C++17)
```

Forms the logical conjunction of the type traits `B...`, effectively performing
a logical AND on the sequence of traits.

The specialization `std::conjunction<B1, ..., BN>` has a public and unambiguous
base that is

- if `sizeof...(B) == 0`, `std::true_type`; otherwise
- the first type `Bi` in `B1, ..., BN` for which `bool(Bi::value) == false`, or
  `BN` if there is no such type.

The member names of the base class, other than `conjunction` and `operator=`,
are not hidden and are unambiguously available in `conjunction`.

Conjunction is short-circuiting: if there is a template type argument `Bi` with
`bool(Bi::value) == false`, then instantiating `conjunction<B1, ..., BN>::value`
does not require the instantiation of `Bj::value` for `j > i`.

The behavior of a program that adds specializations for `std::conjunction` or
`std::conjunction_v` is undefined.

### Template parameters

- **B...** — every template argument `Bi` for which `Bi::value` is instantiated
  must be usable as a base class and define member `value` that is convertible
  to `bool`

### Helper variable template

```cpp
template< class... B >
inline constexpr bool conjunction_v = conjunction<B...>::value;  // (since C++17)
```

### Possible implementation

```cpp
template<class...> struct conjunction : std::true_type {};
template<class B1> struct conjunction<B1> : B1 {};
template<class B1, class... Bn>
struct conjunction<B1, Bn...>
    : std::conditional_t<bool(B1::value), conjunction<Bn...>, B1> {};
```

### Notes

A specialization of `conjunction` does not necessarily inherit from either
`std::true_type` or `std::false_type`: it simply inherits from the first `B`
whose `::value`, explicitly converted to `bool`, is `false`, or from the very
last `B` when all of them convert to `true`. For example,
`std::conjunction<std::integral_constant<int, 2>, std::integral_constant<int,
4>>::value` is `4`.

The short-circuit instantiation differentiates `conjunction` from fold
expressions: a fold expression, like `(... && Bs::value)`, instantiates every
`B` in `Bs`, while `std::conjunction_v<Bs...>` stops instantiation once the
value can be determined. This is particularly useful if the later type is
expensive to instantiate or can cause a hard error when instantiated with the
wrong type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_logical_traits` | 201510L | (C++17) | Logical operator type traits

### Example

```cpp
#include <iostream>
#include <type_traits>

// func is enabled if all Ts... have the same type as T
template<typename T, typename... Ts>
std::enable_if_t<std::conjunction_v<std::is_same<T, Ts>...>>
func(T, Ts...)
{
    std::cout << "all types in pack are T\n";
}

// otherwise
template<typename T, typename... Ts>
std::enable_if_t<!std::conjunction_v<std::is_same<T, Ts>...>>
func(T, Ts...)
{
    std::cout << "not all types in pack are T\n";
}

template<typename T, typename... Ts>
constexpr bool all_types_are_same = std::conjunction_v<std::is_same<T, Ts>...>;

static_assert(all_types_are_same<int, int, int>);
static_assert(not all_types_are_same<int, int&, int>);

int main()
{
    func(1, 2, 3);
    func(1, 2, "hello!");
}
```

Output:

```text
all types in pack are T
not all types in pack are T
```

### See also

- **negation (C++17)** — logical NOT metafunction (class template)
- **disjunction (C++17)** — variadic logical OR metafunction (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/conjunction*
