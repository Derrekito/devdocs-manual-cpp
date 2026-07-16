# std::invoke, std::invoke_r

```cpp
template< class F, class... Args >
std::invoke_result_t<F, Args...>
    invoke( F&& f, Args&&... args ) noexcept(/* see below */);  // (since C++17) (until C++20)
template< class F, class... Args >
constexpr std::invoke_result_t<F, Args...>
    invoke( F&& f, Args&&... args ) noexcept(/* see below */);  // (since C++20)
template< class R, class F, class... Args >
constexpr R invoke_r( F&& f, Args&&... args ) noexcept(/* see below */);  // (2) (since C++23)
```

1) Invoke the Callable object `f` with the parameters `args` as by
   `INVOKE(std::forward<F>(f), std::forward<Args>(args)...)`. This overload
   participates in overload resolution only if `std::is_invocable_v<F, Args...>`
   is `true`.

2) Invoke the Callable object `f` with the parameters `args` as by
   `INVOKE<R>(std::forward<F>(f), std::forward<Args>(args)...)`. This overload
   participates in overload resolution only if `std::is_invocable_r_v<R, F,
   Args...>` is `true`.

### Parameters

- **f** — Callable object to be invoked
- **args** — arguments to pass to `f`

### Return value

1) The value returned by `f`.

2) The value returned by `f`, implicitly converted to `R`, if `R` is not `void`.
   None otherwise.

### Exceptions

1) `noexcept` specification: `noexcept(std::is_nothrow_invocable_v<F, Args...>)`

2) `noexcept` specification: `noexcept(std::is_nothrow_invocable_r_v<R, F,
   Args...>)`

### Possible implementation

```cpp
namespace detail
{
    template<class>
    constexpr bool is_reference_wrapper_v = false;
    template<class U>
    constexpr bool is_reference_wrapper_v<std::reference_wrapper<U>> = true;

    template<class C, class Pointed, class T1, class... Args>
    constexpr decltype(auto) invoke_memptr(Pointed C::* f, T1&& t1, Args&&... args)
    {
        if constexpr (std::is_function_v<Pointed>)
        {
            if constexpr (std::is_base_of_v<C, std::decay_t<T1>>)
                return (std::forward<T1>(t1).*f)(std::forward<Args>(args)...);
            else if constexpr (is_reference_wrapper_v<std::decay_t<T1>>)
                return (t1.get().*f)(std::forward<Args>(args)...);
            else
                return ((*std::forward<T1>(t1)).*f)(std::forward<Args>(args)...);
        }
        else
        {
            static_assert(std::is_object_v<Pointed> && sizeof...(args) == 0);
            if constexpr (std::is_base_of_v<C, std::decay_t<T1>>)
                return std::forward<T1>(t1).*f;
            else if constexpr (is_reference_wrapper_v<std::decay_t<T1>>)
                return t1.get().*f;
            else
                return (*std::forward<T1>(t1)).*f;
        }
    }
} // namespace detail

template<class F, class... Args>
constexpr std::invoke_result_t<F, Args...> invoke(F&& f, Args&&... args)
    noexcept(std::is_nothrow_invocable_v<F, Args...>)
{
    if constexpr (std::is_member_pointer_v<std::decay_t<F>>)
        return detail::invoke_memptr(f, std::forward<Args>(args)...);
    else
        return std::forward<F>(f)(std::forward<Args>(args)...);
}
```

```cpp
template<class R, class F, class... Args>
    requires std::is_invocable_r_v<R, F, Args...>
constexpr R invoke_r(F&& f, Args&&... args)
    noexcept(std::is_nothrow_invocable_r_v<R, F, Args...>)
{
    if constexpr (std::is_void_v<R>)
        std::invoke(std::forward<F>(f), std::forward<Args>(args)...);
    else
        return std::invoke(std::forward<F>(f), std::forward<Args>(args)...);
}
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_invoke` | 201411L | (C++17) | `std::invoke`
  `__cpp_lib_invoke_r` | 202106L | (C++23) | `std::invoke_r`

### Example

```cpp
#include <functional>
#include <iostream>
#include <type_traits>

struct Foo
{
    Foo(int num) : num_(num) {}
    void print_add(int i) const { std::cout << num_ + i << '\n'; }
    int num_;
};

void print_num(int i)
{
    std::cout << i << '\n';
}

struct PrintNum
{
    void operator()(int i) const
    {
        std::cout << i << '\n';
    }
};

int main()
{
    // invoke a free function
    std::invoke(print_num, -9);

    // invoke a lambda
    std::invoke([]() { print_num(42); });

    // invoke a member function
    const Foo foo(314159);
    std::invoke(&Foo::print_add, foo, 1);

    // invoke (access) a data member
    std::cout << "num_: " << std::invoke(&Foo::num_, foo) << '\n';

    // invoke a function object
    std::invoke(PrintNum(), 18);

#if defined(__cpp_lib_invoke_r)
    auto add = [](int x, int y) { return x + y; };
    auto ret = std::invoke_r<float>(add, 11, 22);
    static_assert(std::is_same<decltype(ret), float>());
    std::cout << ret << '\n';
    std::invoke_r<void>(print_num, 44);
#endif
}
```

Possible output:

```text
-9
42
314160
num_: 314159
18
33
44
```

### See also

- **mem_fn (C++11)** — creates a function object out of a pointer to a member
  (function template)
- **result_ofinvoke_result (C++11)(removed in C++20)(C++17)** — deduces the
  result type of invoking a callable object with a set of arguments (class
  template)
- **is_invocableis_invocable_ris_nothrow_invocableis_nothrow_invocable_r
  (C++17)** — checks if a type can be invoked (as if by `std::invoke`) with the
  given argument types (class template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/invoke*
