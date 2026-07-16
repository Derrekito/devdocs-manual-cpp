# std::is_destructible, std::is_trivially_destructible, std::is_nothrow_destructible

```cpp
template< class T >
struct is_destructible;  // (1) (since C++11)
template< class T >
struct is_trivially_destructible;  // (2) (since C++11)
template< class T >
struct is_nothrow_destructible;  // (3) (since C++11)
```

1) If `T` is a reference type, provides the member constant `value` equal to
   `true`. If `T` is (possibly cv-qualified) `void`, a function type, or an
   array of unknown bound, `value` equals `false`. If `T` is an object type,
   then, for the type `U` that is `std::remove_all_extents<T>::type`, if the
   expression `std::declval<U&>().~U()` is well-formed in unevaluated context,
   `value` equals `true`. Otherwise, `value` equals `false`.

2) Same as (1) and additionally `std::remove_all_extents<T>::type` is either a
   non-class type or a class type with a trivial destructor.

3) Same as (1), but the destructor is noexcept.

`T` shall be a complete type, (possibly cv-qualified) void, or an array of
unknown bound. Otherwise, the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Helper variable templates

```cpp
template< class T >
inline constexpr bool is_destructible_v = is_destructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_destructible_v = is_trivially_destructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_destructible_v = is_nothrow_destructible<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is destructible, `false` otherwise (public
  static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Because the C++ program terminates if a destructor throws an exception during
stack unwinding (which usually cannot be predicted), all practical destructors
are non-throwing even if they are not declared noexcept. All destructors found
in the C++ standard library are non-throwing.

Storage occupied by trivially destructible objects may be reused without calling
the destructor.

### Example

```cpp
#include <iostream>
#include <string>
#include <type_traits>

struct Foo
{
    std::string str;
    ~Foo() noexcept {};
};

struct Bar
{
    ~Bar() = default;
};

int main()
{
    std::cout << std::boolalpha
              << "std::string is destructible? "
              << std::is_destructible<std::string>::value << '\n'
              << "Foo is trivially destructible? "
              << std::is_trivially_destructible_v<Foo> << '\n'
              << "Foo is nothrow destructible? "
              << std::is_nothrow_destructible<Foo>() << '\n'
              << "Bar is trivially destructible? "
              << std::is_trivially_destructible<Bar>{} << '\n';
}
```

Output:

```text
std::string is destructible? true
Foo is trivially destructible? false
Foo is nothrow destructible? true
Bar is trivially destructible? true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2049 | C++11 | the specification was incompletable because of the
      imaginary wrapping struct | made complete

### See also

- **is_constructibleis_trivially_constructibleis_nothrow_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a constructor for specific
  arguments (class template)
- **has_virtual_destructor (C++11)** — checks if a type has a virtual destructor
  (class template)
- **destructible (C++20)** — specifies that an object of the type can be
  destroyed (concept)
- **destructor** — releases claimed resources

---
*Source: https://en.cppreference.com/w/cpp/types/is_destructible*
