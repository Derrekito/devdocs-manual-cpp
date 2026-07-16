# std::is_invocable, std::is_invocable_r, std::is_nothrow_invocable, std::is_nothrow_invocable_r

```cpp
template< class Fn, class... ArgTypes >
struct is_invocable;  // (1) (since C++17)
template< class R, class Fn, class... ArgTypes >
struct is_invocable_r;  // (2) (since C++17)
template< class Fn, class... ArgTypes >
struct is_nothrow_invocable;  // (3) (since C++17)
template< class R, class Fn, class... ArgTypes >
struct is_nothrow_invocable_r;  // (4) (since C++17)
```

1) Determines whether `INVOKE(std::declval<Fn>(), std::declval<ArgTypes>()...)`
   is well formed when treated as an unevaluated operand.

2) Determines whether `INVOKE<R>(std::declval<Fn>(),
   std::declval<ArgTypes>()...)` is well formed when treated as an unevaluated
   operand.

3) Determines whether `INVOKE(std::declval<Fn>(), std::declval<ArgTypes>()...)`
   is well formed when treated as an unevaluated operand, and is known not to
   throw any exceptions.

4) Determines whether `INVOKE<R>(std::declval<Fn>(),
   std::declval<ArgTypes>()...)` is well formed when treated as an unevaluated
   operand, and is known not to throw any exceptions.

`Fn, R` and all types in the parameter pack `ArgTypes` shall each be a complete
type, (possibly cv-qualified) void, or an array of unknown bound. Otherwise, the
behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Helper variable templates

```cpp
template< class Fn, class... ArgTypes >
inline constexpr bool is_invocable_v =
    std::is_invocable<Fn, ArgTypes...>::value;  // (1) (since C++17)
template< class R, class Fn, class... ArgTypes >
inline constexpr bool is_invocable_r_v =
    std::is_invocable_r<R, Fn, ArgTypes...>::value;  // (2) (since C++17)
template< class Fn, class... ArgTypes >
inline constexpr bool is_nothrow_invocable_v =
    std::is_nothrow_invocable<Fn, ArgTypes...>::value;  // (3) (since C++17)
template< class R, class Fn, class... ArgTypes >
inline constexpr bool is_nothrow_invocable_r_v =
    std::is_nothrow_invocable_r<R, Fn, ArgTypes...>::value;  // (4) (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if (for overload (1)) `INVOKE(std::declval<Fn>(),
  std::declval<ArgTypes>()...)` is well formed when treated as an unevaluated
  operand, `false` otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_invocable` | 201703L | (C++17) | `std::is_invocable`,
      `std::invoke_result`

### Examples

```cpp
#include <type_traits>

auto func2(char) -> int (*)()
{
    return nullptr;
}

int main()
{
    static_assert(std::is_invocable_v<int()>);
    static_assert(not std::is_invocable_v<int(), int>);
    static_assert(std::is_invocable_r_v<int, int()>);
    static_assert(not std::is_invocable_r_v<int*, int()>);
    static_assert(std::is_invocable_r_v<void, void(int), int>);
    static_assert(not std::is_invocable_r_v<void, void(int), void>);
    static_assert(std::is_invocable_r_v<int(*)(), decltype(func2), char>);
    static_assert(not std::is_invocable_r_v<int(*)(), decltype(func2), void>);
}
```

### See also

- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)
- **result_ofinvoke_result (C++11)(removed in C++20)(C++17)** — deduces the
  result type of invoking a callable object with a set of arguments (class
  template)
- **declval (C++11)** — obtains a reference to its argument for use in
  unevaluated context (function template)
- **invocableregular_invocable (C++20)** — specifies that a callable type can be
  invoked with a given set of argument types (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_invocable*
