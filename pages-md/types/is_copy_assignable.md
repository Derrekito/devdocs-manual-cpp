# std::is_copy_assignable, std::is_trivially_copy_assignable, std::is_nothrow_copy_assignable

```cpp
template< class T >
struct is_copy_assignable;  // (1) (since C++11)
template< class T >
struct is_trivially_copy_assignable;  // (2) (since C++11)
template< class T >
struct is_nothrow_copy_assignable;  // (3) (since C++11)
```

1) If `T` is not a referenceable type (i.e., possibly cv-qualified `void` or a
   function type with a *cv-qualifier-seq* or a *ref-qualifier*), provides a
   member constant `value` equal to `false`. Otherwise, provides a member
   constant `value` equal to `std::is_assignable<T&, const T&>::value`.

2) Same as (1), but uses `std::is_trivially_assignable<T&, const T&>`.

3) Same as (1), but uses `std::is_nothrow_assignable<T&, const T&>`.

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
inline constexpr bool is_copy_assignable_v = is_copy_assignable<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_copy_assignable_v = is_trivially_copy_assignable<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_copy_assignable_v = is_nothrow_copy_assignable<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is copy-assignable, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class T>
struct is_copy_assignable
    : std::is_assignable<typename std::add_lvalue_reference<T>::type,
                         typename std::add_lvalue_reference<const T>::type> {};

template<class T>
struct is_trivially_copy_assignable
    : std::is_trivially_assignable<typename std::add_lvalue_reference<T>::type,
                                   typename std::add_lvalue_reference<const T>::type> {};

template<class T>
struct is_nothrow_copy_assignable
    : std::is_nothrow_assignable<typename std::add_lvalue_reference<T>::type,
                                 typename std::add_lvalue_reference<const T>::type> {};
```

### Notes

The trait `std::is_copy_assignable` is less strict than CopyAssignable because
it does not check the type of the result of the assignment (which, for a
CopyAssignable type, must be an lvalue of type `T`) and does not check the
semantic requirement that the argument expression remains unchanged. It also
does not check that `T` satisfies MoveAssignable, which is required of all
CopyAssignable types.

### Example

```cpp
#include <iostream>
#include <type_traits>
#include <utility>

struct Foo { int n; };

int main()
{
    std::cout << std::boolalpha
              << "Foo is trivially copy-assignable? "
              << std::is_trivially_copy_assignable<Foo>::value << '\n'
              << "int[2] is copy-assignable? "
              << std::is_copy_assignable<int[2]>::value << '\n'
              << "int is nothrow copy-assignable? "
              << std::is_nothrow_copy_assignable<int>::value << '\n';
}
```

Output:

```text
Foo is trivially copy-assignable? true
int[2] is copy-assignable? false
int is nothrow copy-assignable? true
```

### See also

- **is_assignableis_trivially_assignableis_nothrow_assignable
  (C++11)(C++11)(C++11)** — checks if a type has an assignment operator for a
  specific argument (class template)
- **is_move_assignableis_trivially_move_assignableis_nothrow_move_assignable
  (C++11)(C++11)(C++11)** — checks if a type has a move assignment operator
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_copy_assignable*
