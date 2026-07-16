# std::is_function

```cpp
template< class T >
struct is_function;  // (since C++11)
```

`std::is_function` is a UnaryTypeTrait.

Checks whether `T` is a function type. Types like `std::function`, lambdas,
classes with overloaded `operator()` and pointers to functions don't count as
function types. Provides the member constant `value` which is equal to `true`,
if `T` is a function type. Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_function` or
`std::is_function_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_function_v = is_function<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a function type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

`std::is_function` can be implemented in much simpler ways. Implementations
similar to the following one are used by new versions of libc++, libstdc++ and
MS STL:

```cpp
template<class T>
struct is_function : std::integral_constant<
    bool,
    !std::is_const<const T>::value && !std::is_reference<T>::value
> {};
```

The implementation shown below is for pedagogical purposes, since it exhibits
the myriad kinds of function types.

### Possible implementation

```cpp
// primary template
template<class>
struct is_function : std::false_type {};

// specialization for regular functions
template<class Ret, class... Args>
struct is_function<Ret(Args...)> : std::true_type {};

// specialization for variadic functions such as std::printf
template<class Ret, class... Args>
struct is_function<Ret(Args......)> : std::true_type {};

// specialization for function types that have cv-qualifiers
template<class Ret, class... Args>
struct is_function<Ret(Args...) const> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile> : std::true_type {};

// specialization for function types that have ref-qualifiers
template<class Ret, class... Args>
struct is_function<Ret(Args...) &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile &> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile &&> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile &&> : std::true_type {};

// specializations for noexcept versions of all the above (C++17 and later)
template<class Ret, class... Args>
struct is_function<Ret(Args...) noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile & noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) volatile && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args...) const volatile && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) volatile && noexcept> : std::true_type {};
template<class Ret, class... Args>
struct is_function<Ret(Args......) const volatile && noexcept> : std::true_type {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

struct A { int fun() const&; };

template<typename>
struct PM_traits {};

template<class T, class U>
struct PM_traits<U T::*> { using member_type = U; };

int f();

int main()
{
    std::cout << std::boolalpha;
    std::cout << "#1 " << std::is_function_v<A> << '\n';
    std::cout << "#2 " << std::is_function_v<int(int)> << '\n';
    std::cout << "#3 " << std::is_function_v<decltype(f)> << '\n';
    std::cout << "#4 " << std::is_function_v<int> << '\n';

    using T = PM_traits<decltype(&A::fun)>::member_type; // T is int() const&
    std::cout << "#5 " << std::is_function_v<T> << '\n';
}
```

Output:

```text
#1 false
#2 true
#3 true
#4 false
#5 true
```

### See also

- **is_invocableis_invocable_ris_nothrow_invocableis_nothrow_invocable_r
  (C++17)** — checks if a type can be invoked (as if by `std::invoke`) with the
  given argument types (class template)
- **is_object (C++11)** — checks if a type is an object type (class template)
- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_function*
