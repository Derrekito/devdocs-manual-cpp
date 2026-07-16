# std::result_of, std::invoke_result

```cpp
template< class >
class result_of; // not defined

template< class F, class... ArgTypes >
class result_of<F(ArgTypes...)>;  // (1) (since C++11) (deprecated in C++17) (removed in C++20)
template< class F, class... ArgTypes >
class invoke_result;  // (2) (since C++17)
```

Deduces the return type of an `INVOKE` expression at compile time.

`F` must be a callable type, reference to function, or reference to callable
type. Invoking `F` with `ArgTypes...` must be a well-formed expression.
*(since C++11)(until C++14)*

`F` and all types in `ArgTypes` can be any complete type, array of unknown
bound, or (possibly cv-qualified) `void`.
*(since C++14)*

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Member types

- **`type`** — the return type of the Callable type `F` if invoked with the
  arguments `ArgTypes...`. Only defined if F can be called with the arguments
  `ArgTypes...` in unevaluated context.(since C++14)

### Helper types

```cpp
template< class T >
using result_of_t = typename result_of<T>::type;  // (1) (since C++14) (deprecated in C++17) (removed in C++20)
template< class F, class... ArgTypes >
using invoke_result_t = typename invoke_result<F, ArgTypes...>::type;  // (2) (since C++17)
```

### Possible implementation

```cpp
namespace detail
{
    template<class T>
    struct is_reference_wrapper : std::false_type {};
    template<class U>
    struct is_reference_wrapper<std::reference_wrapper<U>> : std::true_type {};

    template<class T>
    struct invoke_impl
    {
        template<class F, class... Args>
        static auto call(F&& f, Args&&... args)
            -> decltype(std::forward<F>(f)(std::forward<Args>(args)...));
    };

    template<class B, class MT>
    struct invoke_impl<MT B::*>
    {
        template<class T, class Td = typename std::decay<T>::type,
            class = typename std::enable_if<std::is_base_of<B, Td>::value>::type>
        static auto get(T&& t) -> T&&;

        template<class T, class Td = typename std::decay<T>::type,
            class = typename std::enable_if<is_reference_wrapper<Td>::value>::type>
        static auto get(T&& t) -> decltype(t.get());

        template<class T, class Td = typename std::decay<T>::type,
            class = typename std::enable_if<!std::is_base_of<B, Td>::value>::type,
            class = typename std::enable_if<!is_reference_wrapper<Td>::value>::type>
        static auto get(T&& t) -> decltype(*std::forward<T>(t));

        template<class T, class... Args, class MT1,
            class = typename std::enable_if<std::is_function<MT1>::value>::type>
        static auto call(MT1 B::*pmf, T&& t, Args&&... args)
            -> decltype((invoke_impl::get(
                std::forward<T>(t)).*pmf)(std::forward<Args>(args)...));

        template<class T>
        static auto call(MT B::*pmd, T&& t)
            -> decltype(invoke_impl::get(std::forward<T>(t)).*pmd);
    };

    template<class F, class... Args, class Fd = typename std::decay<F>::type>
    auto INVOKE(F&& f, Args&&... args)
        -> decltype(invoke_impl<Fd>::call(std::forward<F>(f),
            std::forward<Args>(args)...));
} // namespace detail

// Minimal C++11 implementation:
template<class> struct result_of;
template<class F, class... ArgTypes>
struct result_of<F(ArgTypes...)>
{
    using type = decltype(detail::INVOKE(std::declval<F>(), std::declval<ArgTypes>()...));
};

// Conforming C++14 implementation (is also a valid C++11 implementation):
namespace detail
{
    template<typename AlwaysVoid, typename, typename...>
    struct invoke_result {};
    template<typename F, typename...Args>
    struct invoke_result<
        decltype(void(detail::INVOKE(std::declval<F>(), std::declval<Args>()...))),
            F, Args...>
    {
        using type = decltype(detail::INVOKE(std::declval<F>(), std::declval<Args>()...));
    };
} // namespace detail

template<class> struct result_of;
template<class F, class... ArgTypes>
struct result_of<F(ArgTypes...)> : detail::invoke_result<void, F, ArgTypes...> {};

template<class F, class... ArgTypes>
struct invoke_result : detail::invoke_result<void, F, ArgTypes...> {};
```

### Notes

As formulated in C++11, the behavior of `std::result_of` is undefined when
`INVOKE(std::declval<F>(), std::declval<ArgTypes>()...)` is ill-formed (e.g.
when F is not a callable type at all). C++14 changes that to a SFINAE (when F is
not callable, `std::result_of<F(ArgTypes...)>` simply doesn't have the `type`
member).

The motivation behind `std::result_of` is to determine the result of invoking a
Callable, in particular if that result type is different for different sets of
arguments.

`F(Args...)` is a function type with `Args...` being the argument types and `F`
being the return type. As such, `std::result_of` suffers from several quirks
that led to its deprecation in favor of `std::invoke_result` in C++17:

- `F` cannot be a function type or an array type (but can be a reference to
  them);
- if any of the `Args` has type "array of `T`" or a function type `T`, it is
  automatically adjusted to `T*`;
- neither `F` nor any of `Args...` can be an abstract class type;
- if any of `Args...` has a top-level cv-qualifier, it is discarded;
- none of `Args...` may be of type `void`.

To avoid these quirks, `result_of` is often used with reference types as `F` and
`Args...`. For example:

```cpp
template<class F, class... Args>
std::result_of_t<F&&(Args&&...)> // instead of std::result_of_t<F(Args...)>, which is wrong
    my_invoke(F&& f, Args&&... args)
    {
        /* implementation */
    }
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_result_of_sfinae` | 201210L | (C++14) | `std::result_of` and SFINAE
  `__cpp_lib_is_invocable` | 201703L | (C++17) | `std::is_invocable`,
      `std::invoke_result`

### Examples

```cpp
#include <iostream>
#include <type_traits>

struct S
{
    double operator()(char, int&);
    float operator()(int) { return 1.0; }
};

template<class T>
typename std::result_of<T(int)>::type f(T& t)
{
    std::cout << "overload of f for callable T\n";
    return t(0);
}

template<class T, class U>
int f(U u)
{
    std::cout << "overload of f for non-callable T\n";
    return u;
}

int main()
{
    // the result of invoking S with char and int& arguments is double
    std::result_of<S(char, int&)>::type d = 3.14; // d has type double
    static_assert(std::is_same<decltype(d), double>::value, "");

    // std::invoke_result uses different syntax (no parentheses)
    std::invoke_result<S,char,int&>::type b = 3.14;
    static_assert(std::is_same<decltype(b), double>::value, "");

    // the result of invoking S with int argument is float
    std::result_of<S(int)>::type x = 3.14; // x has type float
    static_assert(std::is_same<decltype(x), float>::value, "");

    // result_of can be used with a pointer to member function as follows
    struct C { double Func(char, int&); };
    std::result_of<decltype(&C::Func)(C, char, int&)>::type g = 3.14;
    static_assert(std::is_same<decltype(g), double>::value, "");

    f<C>(1); // may fail to compile in C++11; calls the non-callable overload in C++14
}
```

Output:

```text
overload of f for non-callable T
```

### See also

- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)
- **is_invocableis_invocable_ris_nothrow_invocableis_nothrow_invocable_r
  (C++17)** — checks if a type can be invoked (as if by `std::invoke`) with the
  given argument types (class template)
- **declval (C++11)** — obtains a reference to its argument for use in
  unevaluated context (function template)

---
*Source: https://en.cppreference.com/w/cpp/types/result_of*
