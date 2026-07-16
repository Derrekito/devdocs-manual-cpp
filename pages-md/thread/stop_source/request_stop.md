# std::stop_source::request_stop

```cpp
bool request_stop() noexcept;  // (since C++20)
```

Issues a stop request to the stop-state, if the `stop_source` object has a
stop-state and it has not yet already had stop requested.

The determination is made atomically, and if stop was requested, the stop-state
is atomically updated to avoid race conditions, such that:

- `stop_requested()` and `stop_possible()` can be concurrently invoked on other
  `stop_token`s and `stop_source`s of the same stop-state;
- `request_stop()` can be concurrently invoked on other `stop_source` objects,
  and only one will actually perform the stop request.

However, see the Notes section.

### Parameters

(none)

### Return value

`true` if the `stop_source` object has a stop-state and this invocation made a
stop request, otherwise `false`.

### Postconditions

`stop_possible()` is `false` or `stop_requested()` is `true`.

### Notes

If the `request_stop()` does issue a stop request (i.e., returns `true`), then
any `stop_callback`s registered for the same associated stop-state will be
invoked synchronously, on the same thread `request_stop()` is issued on. If an
invocation of a callback exits via an exception, `std::terminate` is called.

If the `stop_source` object has a stop-state but a stop request has already been
made, this function returns `false`. However there is no guarantee that another
`stop_source` object which has just (successfully) requested stop is not still
in the middle of invoking a `stop_callback` function.

If the `request_stop()` does issue a stop request (i.e., returns `true`), then
all condition variables of base type `std::condition_variable_any` registered
with an interruptible wait for `stop_token`s associated with the `stop_source`'s
stop-state will be notified.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/request_stop*
