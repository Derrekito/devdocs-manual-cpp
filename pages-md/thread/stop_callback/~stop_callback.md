# std::stop_callback<Callback>::~stop_callback

```cpp
~stop_callback();  // (since C++20)
```

Destroys the `stop_callback` object.

If `*this` has a `stop_token` with associated stop-state, deregisters the
callback from it.

If the callback function is being invoked concurrently on another thread, the
destructor does not complete until the callback function invocation is complete.
If the callback function is being invoked on the same thread the destructor is
being invoked on, then the destructor returns without waiting for callback
invocation to complete (see Notes).

### Notes

The `stop_callback` destructor is designed to prevent race conditions and
deadlocks. If another thread is currently invoking the callback, then the
destructor cannot return until that completes, or else the function object could
potentially be destroyed while it is being executed. The callback function is
not required to be neither copyable nor movable - it lives in the
`stop_callback` object itself even after registration.

On the other hand, if the current thread invoking the destructor is the same
thread that is invoking the callback, then the destructor cannot wait or else a
deadlock would occur. It is possible and valid for the same thread to be
destroying the `stop_callback` while it is invoking its callback function,
because the callback function might itself destroy the `stop_callback`, directly
or indirectly.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_callback/%7Estop_callback*
