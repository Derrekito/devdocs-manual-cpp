# std::is_move_assignable, std::is_trivially_move_assignable, std::is_nothrow_move_assignable

```cpp
template< class T >
struct is_move_assignable;  // (1) (since C++11)
template< class T >
struct is_trivially_move_assignable;  // (2) (since C++11)
template< class T >
struct is_nothrow_move_assignable;  // (3) (since C++11)
```

1) If `T` is not a referenceable type (i.e., possibly cv-qualified `void` or a
   function type with a *cv-qualifier-seq* or a *ref-qualifier*), provides a
   member constant `value` equal to `false`. Otherwise, provides a member
   constant `value` equal to `std::is_assignable<T&, T&&>::value`.

2) Same as (1), but uses `std::is_trivially_assignable<T&, T&&>`.

3) Same as (1), but uses `std::is_nothrow_assignable<T&, T&&>`.

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
inline constexpr bool is_move_assignable_v =
    is_move_assignable<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_move_assignable_v =
    is_trivially_move_assignable<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_move_assignable_v =
    is_nothrow_move_assignable<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is move-assignable, `false` otherwise
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
struct is_move_assignable
    : std::is_assignable<typename std::add_lvalue_reference<T>::type,
                         typename std::add_rvalue_reference<T>::type> {};

template<class T>
struct is_trivially_move_assignable
    : std::is_trivially_assignable<typename std::add_lvalue_reference<T>::type,
                                   typename std::add_rvalue_reference<T>::type> {};

template<class T>
struct is_nothrow_move_assignable
    : std::is_nothrow_assignable<typename std::add_lvalue_reference<T>::type,
                                 typename std::add_rvalue_reference<T>::type> {};
```

### Notes

The trait `std::is_move_assignable` is less strict than MoveAssignable because
it does not check the type of the result of the assignment (which, for a
MoveAssignable type, must be `T&`), nor the semantic requirement that the
target's value after the assignment is equivalent to the source's value before
the assignment.

The type does not have to implement a move assignment operator in order to
satisfy this trait; see MoveAssignable for details.

### Example

```cpp
#include <iostream>
#include <string>
#include <type_traits>

struct Foo { int n; };

struct NoMove
{
    // prevents implicit declaration of default move assignment operator
    // however, the class is still move-assignable because its
    // copy assignment operator can bind to an rvalue argument
    NoMove& operator=(const NoMove&) { return *this; }
};

int main()
{
    std::cout << std::boolalpha
              << "std::string is nothrow move-assignable? "
              << std::is_nothrow_move_assignable<std::string>::value << '\n'
              << "int[2] is move-assignable? "
              << std::is_move_assignable<int[2]>::value << '\n'
              << "Foo is trivially move-assignable? "
              << std::is_trivially_move_assignable<Foo>::value << '\n'
              << "NoMove is move-assignable? "
              << std::is_move_assignable<NoMove>::value << '\n'
              << "NoMove is nothrow move-assignable? "
              << std::is_nothrow_move_assignable<NoMove>::value << '\n';
}
```

Output:

```text
std::string is nothrow move-assignable? true
int[2] is move-assignable? false
Foo is trivially move-assignable? true
NoMove is move-assignable? true
NoMove is nothrow move-assignable? false
```

### See also

- **is_assignableis_trivially_assignableis_nothrow_assignable
  (C++11)(C++11)(C++11)** — checks if a type has an assignment operator for a
  specific argument (class template)
- **is_copy_assignableis_trivially_copy_assignableis_nothrow_copy_assignable
  (C++11)(C++11)(C++11)** — checks if a type has a copy assignment operator
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_move_assignable*
