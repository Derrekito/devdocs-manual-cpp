# std::not_fn

```cpp
template< class F >
/* unspecified */ not_fn( F&& f );  // (1) (since C++17) (constexpr since C++20)
template< auto ConstFn >
constexpr /* unspecified */ not_fn() noexcept;  // (2) (since C++26)
```

1) Creates a forwarding call wrapper that returns the negation of the callable
   object it holds.

2) Creates a forwarding call wrapper that returns the negation of the statically
   determined callable target. The program is ill-formed if `ConstFn` is a null
   pointer or null pointer-to-member.

### Parameters

- **f** — the object from which the Callable object held by the wrapper is
  constructed

**Type requirements**

**-`std::decay_t<F>` must meet the requirements of Callable and MoveConstructible.**

**-`std::is_constructible_v<std::decay_t<F>, F>` is required to be `true`.**

### Return value

1) A function object of unspecified type `T`. It has the following members.
   std::not_fn *return type* Member objects The return type of `std::not_fn`
   holds a member object of type std::decay_t<F>. Constructors explicit T( F&& f
   ); (1) (since C++17) (constexpr since C++20) (exposition only*) T( T&& f ) =
   default; T( const T& f ) = default; (2) (since C++17) 1) The constructor
   direct-non-list-initializes the member object (of type `std::decay_t<F>`)
   from `std::forward<F>(f)`. Throws any exception thrown by the constructor
   selected. 2) Because `std::decay_t<F>` is required to be MoveConstructible,
   the returned call wrapper is always MoveConstructible, and is
   CopyConstructible if `std::decay_t<F>` is CopyConstructible. The explicitly
   defaulted definitions make the return type not assignable. It is unspecified
   whether these constructors are explicitly defaulted and whether the return
   type is assignable. Member function operator() (1) template< class... Args >
   auto operator()( Args&&... args ) & -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F>&, Args...>>()); template< class... Args
   > auto operator()( Args&&... args ) const& -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F> const&, Args...>>()); (since C++17)
   (until C++20) template< class... Args > constexpr auto operator()( Args&&...
   args ) & noexcept(/* see below */) -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F>&, Args...>>()); template< class... Args
   > constexpr auto operator()( Args&&... args ) const& noexcept(/* see below
   */) -> decltype(!std::declval< std::invoke_result_t<std::decay_t<F> const&,
   Args...>>()); (since C++20) (2) template< class... Args > auto operator()(
   Args&&... args ) && -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F>, Args...>>()); template< class... Args >
   auto operator()( Args&&... args ) const&& -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F> const, Args...>>()); (since C++17)
   (until C++20) template< class... Args > constexpr auto operator()( Args&&...
   args ) && noexcept(/* see below */) -> decltype(!std::declval<
   std::invoke_result_t<std::decay_t<F>, Args...>>()); template< class... Args >
   constexpr auto operator()( Args&&... args ) const&& noexcept(/* see below */)
   -> decltype(!std::declval< std::invoke_result_t<std::decay_t<F> const,
   Args...>>()); (since C++20) Let `fd` be the member object of type
   std::decay_t<F>. 1) Equivalent to `return !std::invoke(fd,
   std::forward<Args>(args)...);` 2) Equivalent to `return
   !std::invoke(std::move(fd), std::forward<Args>(args)...);` While invoking the
   result, if the substitution into the return type of the originally selected
   operator() overload fails, another overload may be selected. (since C++17)
   (until C++20) 1) Expression-equivalent to `!std::invoke(fd,
   std::forward<Args>(args)...)` 2) Expression-equivalent to
   `!std::invoke(std::move(fd), std::forward<Args>(args)...)` While invoking the
   result, if the substitution into the return type of the originally selected
   operator() overload fails, the invocation is ill-formed, which can also be a
   substitution failure. (since C++20)

2) A value of the following type. std::not_fn *stateless return type* The return
   type is a CopyConstructible stateless class. It is unspecified whether the
   return type is assignable. Member function operator() template< class... Args
   > constexpr auto operator()( Args&&... args ) const noexcept(/* see below */)
   -> decltype( !std::declval<std::invoke_result_t<decltype((ConstFn)),
   Args...>>()); (since C++26) Expression-equivalent to `!std::invoke(ConstFn,
   std::forward<Args>(args)...)`.

### Exceptions

1) Throws no exceptions, unless the construction of `fd` throws.

### Possible implementation

```cpp
namespace detail
{
    template<class V, class F, class... Args>
    constexpr bool negate_invocable_impl = false;
    template<class F, class... Args>
    constexpr bool negate_invocable_impl<std::void_t<decltype(
        !std::invoke(std::declval<F>(), std::declval<Args>()...))>, F, Args...> = true;

    template<class F, class... Args>
    constexpr bool negate_invocable_v = negate_invocable_impl<void, F, Args...>;

    template<class F>
    struct not_fn_t
    {
        F f;

        template<class... Args,
            std::enable_if_t<negate_invocable_v<F&, Args...>, int> = 0>
        constexpr decltype(auto) operator()(Args&&... args) &
            noexcept(noexcept(!std::invoke(f, std::forward<Args>(args)...)))
        {
            return !std::invoke(f, std::forward<Args>(args)...);
        }

        template<class... Args,
            std::enable_if_t<negate_invocable_v<const F&, Args...>, int> = 0>
        constexpr decltype(auto) operator()(Args&&... args) const&
            noexcept(noexcept(!std::invoke(f, std::forward<Args>(args)...)))
        {
            return !std::invoke(f, std::forward<Args>(args)...);
        }

        template<class... Args,
            std::enable_if_t<negate_invocable_v<F, Args...>, int> = 0>
        constexpr decltype(auto) operator()(Args&&... args) &&
            noexcept(noexcept(!std::invoke(std::move(f), std::forward<Args>(args)...)))
        {
            return !std::invoke(std::move(f), std::forward<Args>(args)...);
        }

        template<class... Args,
            std::enable_if_t<negate_invocable_v<const F, Args...>, int> = 0>
        constexpr decltype(auto) operator()(Args&&... args) const&&
            noexcept(noexcept(!std::invoke(std::move(f), std::forward<Args>(args)...)))
        {
            return !std::invoke(std::move(f), std::forward<Args>(args)...);
        }

        // Deleted overloads are needed since C++20
        // for preventing a non-equivalent but well-formed overload to be selected.

        template<class... Args,
            std::enable_if_t<!negate_invocable_v<F&, Args...>, int> = 0>
        void operator()(Args&&...) & = delete;

        template<class... Args,
            std::enable_if_t<!negate_invocable_v<const F&, Args...>, int> = 0>
        void operator()(Args&&...) const& = delete;

        template<class... Args,
            std::enable_if_t<!negate_invocable_v<F, Args...>, int> = 0>
        void operator()(Args&&...) && = delete;

        template<class... Args,
            std::enable_if_t<!negate_invocable_v<const F, Args...>, int> = 0>
        void operator()(Args&&...) const&& = delete;
    };
}

template<class F>
constexpr detail::not_fn_t<std::decay_t<F>> not_fn(F&& f)
{
    return {std::forward<F>(f)};
}
```

```cpp
namespace detail
{
    template<auto ConstFn>
    struct stateless_not_fn
    {
        template<class... Args>
        constexpr auto operator()(Args&&... args) const
            noexcept(noexcept(!std::invoke(ConstFn, std::forward<Args>(args)...)))
            -> decltype(!std::invoke(ConstFn, std::forward<Args>(args)...))
        {
            return !std::invoke(ConstFn, std::forward<Args>(args)...);
        }
    };
}

template<auto ConstFn>
constexpr detail::stateless_not_fn<ConstFn> not_fn() noexcept
{
    if constexpr (std::is_pointer_v<decltype(ConstFn)> ||
                  std::is_member_pointer_v<decltype(ConstFn)>)
        static_assert(ConstFn != nullptr);

    return {};
}
```

### Notes

`std::not_fn` is intended to replace the C++03-era negators `std::not1` and
`std::not2`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_not_fn` | 201603L | (C++17) | `std::not_fn()`, (1)
  202306L | (C++26) | Allow passing callable objects as non-type template
      arguments to `std::not_fn`, (2)

### Example

```cpp
#include <cassert>
#include <functional>

bool is_same(int a, int b) noexcept
{
    return a == b;
}

struct S
{
    int val;
    bool is_same(int arg) const noexcept { return val == arg; }
};

int main()
{
    // Using with a free function:
    auto is_differ = std::not_fn(is_same);
    assert(is_differ(8, 8) == false); // equivalent to: !is_same(8, 8) == false
    assert(is_differ(6, 9) == true); // equivalent to: !is_same(8, 0) == true

    // Using with a member function:
    auto member_differ = std::not_fn(&S::is_same);
    assert(member_differ(S{3}, 3) == false); //: S tmp{6}; !tmp.is_same(6) == false

    // Noexcept-specification is preserved:
    static_assert(noexcept(is_differ) == noexcept(is_same));
    static_assert(noexcept(member_differ) == noexcept(&S::is_same));

    // Using with a function object:
    auto same = [](int a, int b) { return a == b; };
    auto differ = std::not_fn(same);
    assert(differ(1, 2) == true); //: !same(1, 2) == true
    assert(differ(2, 2) == false); //: !same(2, 2) == false

#if __cpp_lib_not_fn >= 202306L
    auto is_differ_cpp26 = std::not_fn<is_same>();
    assert(is_differ_cpp26(8, 8) == false);
    assert(is_differ_cpp26(6, 9) == true);

    auto member_differ_cpp26 = std::not_fn<&S::is_same>();
    assert(member_differ_cpp26(S{3}, 3) == false);

    auto differ_cpp26 = std::not_fn<same>();
    static_assert(differ_cpp26(1, 2) == true);
    static_assert(differ_cpp26(2, 2) == false);
#endif
}
```

### See also

- **not1 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::unary_negate` object (function template)
- **not2 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::binary_negate` object (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/not_fn*
