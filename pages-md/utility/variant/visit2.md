# std::variant<Types...>::visit

```cpp
template< class Self, class Visitor >
constexpr decltype(auto) visit( this Self&& self, Visitor&& vis );  // (1) (since C++26)
template< class R, class Self, class Visitor >
constexpr R visit( this Self&& self, Visitor&& vis );  // (2) (since C++26)
```

Applies the visitor `vis` (a Callable that can be called with any combination of
types from the variant) to the variant held by `self`.

Given type `V` as `decltype(std::forward_like<Self>(std::declval<variant>()))`,
the equivalent call is:

1) `return std::visit(std::forward<Visitor>(vis), (V) self);`.

2) `return std::visit<R>(std::forward<Visitor>(vis), (V) self);`.

### Parameters

- **vis** — a Callable that accepts every possible alternative from the variant
- **self** — variant to pass to the visitor

### Return value

1) The result of the `std::visit` invocation.

2) Nothing if `R` is (possibly cv-qualified) `void`; otherwise the result of the
   `std::visit<R>` invocation.

### Exceptions

Only throws if the call to `std::visit` throws.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_variant` | 202306L | (C++26) | member `visit`

### Example

```cpp
#include <iostream>
#include <string>
#include <variant>

// helper type for the visitor
template<class... Ts>
struct overloads : Ts... { using Ts::operator()...; };

int main()
{
    std::variant<int, std::string> var1{42}, var2{"abc"};

    auto use_int = [](int i){ std::cout << "int = " << i << '\n'; };
    auto use_str = [](std::string s){ std::cout << "string = " << s << '\n'; };

#if (__cpp_lib_variant >= 202306L)
    var1.visit(overloads{use_int, use_str});
    var2.visit(overloads{use_int, use_str});
#else
    std::visit(overloads{use_int, use_str}, var1);
    std::visit(overloads{use_int, use_str}, var2);
#endif
}
```

Output:

```text
int = 42
string = abc
```

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/visit2*
