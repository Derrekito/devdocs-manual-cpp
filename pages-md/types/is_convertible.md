# std::is_convertible, std::is_nothrow_convertible

```cpp
template< class From, class To >
struct is_convertible;  // (1) (since C++11)
template< class From, class To >
struct is_nothrow_convertible;  // (2) (since C++20)
```

1) If the imaginary function definition `To test() { return
   std::declval<From>(); }` is well-formed, (that is, either
   `std::declval<From>()` can be converted to `To` using implicit conversions,
   or both `From` and `To` are possibly cv-qualified `void`), provides the
   member constant `value` equal to `true`. Otherwise `value` is `false`. For
   the purposes of this check, the use of `std::declval` in the return statement
   is not considered an odr-use.

Access checks are performed as if from a context unrelated to either type. Only
   the validity of the immediate context of the expression in the return
   statement (including conversions to the return type) is considered.

2) Same as (1), but the conversion is also `noexcept`.

`From` and `To` shall each be a complete type, (possibly cv-qualified) void, or
an array of unknown bound. Otherwise, the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Helper variable template

```cpp
template< class From, class To >
inline constexpr bool is_convertible_v = is_convertible<From, To>::value;  // (since C++17)
template< class From, class To >
inline constexpr bool is_nothrow_convertible_v = is_nothrow_convertible<From, To>::value;  // (since C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `From` is convertible to `To` , `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
namespace detail
{
    template<class T>
    auto test_returnable(int) -> decltype(
        void(static_cast<T(*)()>(nullptr)), std::true_type{}
    );
    template<class>
    auto test_returnable(...) -> std::false_type;

    template<class From, class To>
    auto test_implicitly_convertible(int) -> decltype(
        void(std::declval<void(&)(To)>()(std::declval<From>())), std::true_type{}
    );
    template<class, class>
    auto test_implicitly_convertible(...) -> std::false_type;
} // namespace detail

template<class From, class To>
struct is_convertible : std::integral_constant<bool,
    (decltype(detail::test_returnable<To>(0))::value &&
     decltype(detail::test_implicitly_convertible<From, To>(0))::value) ||
    (std::is_void<From>::value && std::is_void<To>::value)
> {};
```

```cpp
template<class From, class To>
struct is_nothrow_convertible : std::conjunction<std::is_void<From>, std::is_void<To>> {};

template<class From, class To>
    requires
        requires
        {
            static_cast<To(*)()>(nullptr);
            { std::declval<void(&)(To) noexcept>()(std::declval<From>()) } noexcept;
        }
struct is_nothrow_convertible<From, To> : std::true_type {};
```

### Notes

Gives well-defined results for reference types, void types, array types, and
function types.

Currently the standard has not specified whether the destruction of the object
produced by the conversion (either a result object or a temporary bound to a
reference) is considered as a part of the conversion. This is LWG issue 3400.

All known implementations treat the destruction as a part of the conversion, as
proposed in P0758R1.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_nothrow_convertible` | 201806L | (C++20) |
      `std::is_nothrow_convertible`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <string_view>
#include <type_traits>

class E { public: template<class T> E(T&&) {} };

int main()
{
    class A {};
    class B : public A {};
    class C {};
    class D { public: operator C() { return c; } C c; };

    std::cout
        << std::boolalpha
        << std::is_convertible_v<B*, A*> << ' ' // true
        << std::is_convertible_v<A*, B*> << ' ' // false
        << std::is_convertible_v<D, C> << ' '   // true
        << std::is_convertible_v<B*, C*> << ' ' // false
        // Note that the Perfect Forwarding constructor makes the class E be
        // "convertible" from everything. So, A is replaceable by B, C, D..:
        << std::is_convertible_v<A, E> << ' ';  // true

    using std::operator "" s, std::operator "" sv;

    auto stringify = []<typename T>(T x)
    {
        if constexpr (std::is_convertible_v<T, std::string> or
                      std::is_convertible_v<T, std::string_view>)
            return x;
        else
            return std::to_string(x);
    };

    const char* three = "three";

    std::cout
        << std::is_convertible_v<std::string_view, std::string> << ' ' // false
        << std::is_convertible_v<std::string, std::string_view> << ' ' // true
        << std::quoted(stringify("one"s)) << ' '
        << std::quoted(stringify("two"sv)) << ' '
        << std::quoted(stringify(three)) << ' '
        << std::quoted(stringify(42)) << ' '
        << std::quoted(stringify(42.0)) << '\n';
}
```

Output:

```text
true false true false true false true "one" "two" "three" "42" "42.000000"
```

### See also

- **is_base_of (C++11)** — checks if a type is derived from the other type
  (class template)
- **is_pointer_interconvertible_base_of (C++20)** — checks if a type is a
  *pointer-interconvertible* (initial) base of another type (class template)
- **is_pointer_interconvertible_with_class (C++20)** — checks if objects of a
  type are pointer-interconvertible with the specified subobject of that type
  (function template)
- **convertible_to (C++20)** — specifies that a type is implicitly convertible
  to another type (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_convertible*
