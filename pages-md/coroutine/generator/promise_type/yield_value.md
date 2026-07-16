# std::generator<Ref,V,Allocator>::promise_type::yield_value

```cpp
std::suspend_always yield_value( yielded val ) noexcept;  // (1) (since C++23)
auto yield_value( const std::remove_reference_t<yielded>& lval )
    requires std::is_rvalue_reference_v<yielded> &&
             std::constructible_from<std::remove_cvref_t<yielded>,
                 const std::remove_reference_t<yielded>&>;  // (2) (since C++23)
template< class R2, class V2, class Alloc2, class Unused >
    requires std::same_as<typename std::generator<T2, V2, Alloc2>::yielded, yielded>
auto yield_value( ranges::elements_of<std::generator<T2, V2, Alloc2>&&,
                                      Unused> g ) noexcept;  // (3) (since C++23)
template< ranges::input_range R, class Alloc >
    requires std::convertible_to<ranges::range_reference_t<R>, yielded>
auto yield_value( ranges::elements_of<R, Alloc> r ) noexcept;  // (4) (since C++23)
```

An implementation of coroutine interface functions used internally to support
`operator co_yield`.

(`yielded` is a reference type defined in `std::generator`.)

1) Assigns `std::addressof(val)` to `value_`. Returns `{}`.

2) Returns an awaitable object of an unspecified type that stores an object of
   type `std::remove_cvref_t<yielded>` direct-non-list-initialized with `lval`,
   whose member functions are configures so that `value_` points to that stored
   object. Then suspends the coroutine.
3) Let `x` be some generator object.

Returns an awaitable object of an unspecified type into which `g.range` is
moved,

- whose member `await_ready` returns false,
- whose member `await_suspend` pushes `g.range.coroutine_` into `*x.active_` and
- resumes execution of the coroutine referred to by `g.range.coroutine_`, and
- whose member `await_resume` evaluates

The coroutine referred to by `g.range.coroutine_` must be suspended at its
   initial suspend point. Otherwise the behavior is undefined.

4) Equivalent to: auto nested = [](std::allocator_arg_t, Alloc,
   ranges::iterator_t<R> i, ranges::sentinel_t<R> s) -> std::generator<yielded,
   ranges::range_value_t<R>, Alloc> { for (; i != s; ++i) co_yield
   static_cast<yielded>(*i); }; return yield_value(ranges::elements_of(nested(
   allocator_arg, r.allocator, ranges::begin(r.range), ranges::end(r.range))));

2,3) A handle referring to the coroutine whose promise object is `*this` must be
   at the top of `*active_` of some generator object. Otherwise the behavior is
   undefined.

2-4) A *yield-expression* that calls these overload has the type `void`.

### Parameters

- **val** — a value which is a result of the *yield-expression* evaluation
- **lval** — an lvalue which is a result of the *yield-expression* evaluation
- **g** — a range of elements produced by a generator
- **r** — a range of elements

### Return value

1) The awaitable object of type `std::suspend_always`.

2-4) An awaitable object of an unspecified type as described above.

### Exceptions

2) May throw any exception thrown by the initialization of the stored object.

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/yield_value*
