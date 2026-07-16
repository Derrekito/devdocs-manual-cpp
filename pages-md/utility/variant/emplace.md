# std::variant<Types...>::emplace

```cpp
template <class T, class... Args>
T& emplace(Args&&... args);  // (since C++17) (until C++20)
template <class T, class... Args>
constexpr T& emplace(Args&&... args);  // (since C++20)
template <class T, class U, class... Args>
T& emplace( std::initializer_list<U> il, Args&&... args );  // (since C++17) (until C++20)
template <class T, class U, class... Args>
constexpr T& emplace( std::initializer_list<U> il, Args&&... args );  // (since C++20)
template <std::size_t I, class... Args>
std::variant_alternative_t<I, variant>& emplace( Args&&... args );  // (since C++17) (until C++20)
template <std::size_t I, class... Args>
constexpr std::variant_alternative_t<I, variant>& emplace( Args&&... args );  // (since C++20)
template <std::size_t I, class U, class... Args>
std::variant_alternative_t<I, variant>&
    emplace( std::initializer_list<U> il, Args&&... args );  // (since C++17) (until C++20)
template <std::size_t I, class U, class... Args>
constexpr std::variant_alternative_t<I, variant>&
    emplace( std::initializer_list<U> il, Args&&... args );  // (since C++20)
```

Creates a new value in-place, in an existing `variant` object

1) Equivalent to `emplace<I>(std::forward<Args>(args)...)`, where `I` is the
zero-based index of `T` in `Types...`.

- This overload participates in overload resolution only if
  `std::is_constructible_v<T, Args...>` is `true`, and `T` occurs exactly once
  in `Types...`

2) Equivalent to `emplace<I>(il, std::forward<Args>(args)...)`, where `I` is the
zero-based index of `T` in `Types...`.

- This overload participates in overload resolution only if
  `std::is_constructible_v<T, std::initializer_list<U>&, Args...>` is `true`,
  and `T` occurs exactly once in `Types...`

3) First, destroys the currently contained value (if any). Then
direct-initializes the contained value as if constructing a value of type `T_I`
with the arguments `std::forward<Args>(args)...`. If an exception is thrown,
`*this` may become valueless_by_exception.

- This overload participates in overload resolution only if
  `std::is_constructible_v<T_I, Args...>` is `true`.
- It is a compile-time error if `I` is not less than `sizeof...(Types)`.

4) First, destroys the currently contained value (if any). Then
direct-initializes the contained value as if constructing a value of type `T_I`
with the arguments `il, std::forward<Args>(args)...`. If an exception is thrown,
`*this` may become valueless_by_exception.

- This overload participates in overload resolution only if
  `std::is_constructible_v<T_I, std::initializer_list<U>&, Args...>` is `true`.
- It is a compile-time error if `I` is not less than `sizeof...(Types)`.

### Parameters

- **args** — constructor arguments to use when constructing the new value
- **il** — initializer_list argument to use when constructing the new value

### Return value

A reference to the new contained value.

### Exceptions

1-4) Any exception thrown during the initialization of the contained value.

### Example

```cpp
#include <iostream>
#include <string>
#include <variant>

int main()
{
    std::variant<std::string> v1;
    v1.emplace<0>("abc"); // OK
    std::cout << std::get<0>(v1) << '\n';
    v1.emplace<std::string>("def"); // OK
    std::cout << std::get<0>(v1) << '\n';

    std::variant<std::string, std::string> v2;
    v2.emplace<1>("ghi"); // OK
    std::cout << std::get<1>(v2) << '\n';
    // v2.emplace<std::string>("abc"); -> Error
}
```

Output:

```text
abc
def
ghi
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2231R1 | C++20 | `emplace` was not constexpr while the required operations
      can be constexpr in C++20 | made constexpr

### See also

- **operator=** — assigns a variant (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/emplace*
