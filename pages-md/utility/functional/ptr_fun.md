# std::ptr_fun

```cpp
template< class Arg, class Result >
std::pointer_to_unary_function<Arg,Result>
    ptr_fun( Result (*f)(Arg) );  // (1) (deprecated in C++11) (removed in C++17)
template< class Arg1, class Arg2, class Result >
std::pointer_to_binary_function<Arg1,Arg2,Result>
    ptr_fun( Result (*f)(Arg1, Arg2) );  // (2) (deprecated in C++11) (removed in C++17)
```

Creates a function wrapper object (either `std::pointer_to_unary_function` or
`std::pointer_to_binary_function`), deducing the target type from the template
arguments.

1) Effectively calls `std::pointer_to_unary_function<Arg,Result>(f)`.

2) Effectively calls `std::pointer_to_binary_function<Arg1,Arg2,Result>(f)`.

This function and the related types are deprecated as of C++11 in favor of the
more general `std::function` and `std::ref`, both of which create callable
adaptor-compatible function objects from plain functions.

### Parameters

- **f** — pointer to a function to create a wrapper for

### Return value

A function object wrapping `f`.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <string_view>

constexpr bool is_vowel(char c)
{
    return std::string_view{"aeoiuAEIOU"}.find(c) != std::string_view::npos;
}

int main()
{
    std::string_view s = "Hello, world!";
    std::ranges::copy_if(s, std::ostreambuf_iterator<char>(std::cout),
        std::not1(std::ptr_fun(is_vowel)));
#if 0
// C++11 alternatives:
        std::not1(std::cref(is_vowel)));
        std::not1(std::function<bool(char)>(is_vowel)));
        [](char c) { return !is_vowel(c); });
// C++17 alternatives:
        std::not_fn(is_vowel));
#endif
}
```

Output:

```text
Hll, wrld!
```

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)
- **not_fn (C++17)** — creates a function object that returns the complement of
  the result of the function object it holds (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ptr_fun*
