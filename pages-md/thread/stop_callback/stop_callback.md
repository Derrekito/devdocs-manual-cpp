# std::stop_callback<Callback>::stop_callback

```cpp
template< class C >
explicit stop_callback( const std::stop_token& st, C&& cb ) noexcept(/*see below*/);  // (1) (since C++20)
template< class C >
explicit stop_callback( std::stop_token&& st, C&& cb ) noexcept(/*see below*/);  // (2) (since C++20)
stop_callback( const stop_callback& ) = delete;  // (3) (since C++20)
stop_callback( stop_callback&& ) = delete;  // (4) (since C++20)
```

Constructs a new `stop_callback` object, saving and registering the `cb`
callback function into the given `std::stop_token`'s associated stop-state, for
later invocation if stop is requested on the associated `std::stop_source`.

1) Constructs a `stop_callback` for the given `st` `std::stop_token` (copied),
   with the given invocable callback function `cb`.

2) Constructs a `stop_callback` for the given `st` `std::stop_token` (moved),
   with the given invocable callback function `cb`.

3,4) `stop_callback` is neither CopyConstructible nor MoveConstructible.

Both constructors participate overload resolution only if `Callback` and `C`
satisfy `constructible_from` of `std::constructible_from<Callback, C>`. If
`Callback` and `C` satisfy the concept but fail to satisfy its semantic
requirement, the behavior is undefined.

### Parameters

- **st** — a `std::stop_token` object to register this `stop_callback` object
  with
- **cb** — the type to invoke if stop is requested

### Exceptions

1,2) `noexcept` specification:
   `noexcept(std::is_nothrow_constructible_v<Callback, C>)` Any exception thrown
   by constructor-initializing the given callback into the `stop_callback`
   object.

### Notes

If `st.stop_requested() == true` for the passed-in `std::stop_token`, then the
callback function is invoked in the current thread before the constructor
returns.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_callback/stop_callback*
