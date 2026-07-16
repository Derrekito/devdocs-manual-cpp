# std::counting_semaphore<LeastMaxValue>::release

```cpp
void release( std::ptrdiff_t update = 1 );  // (since C++20)
```

Atomically increments the internal counter by the value of `update`. Any
thread(s) waiting for the counter to be greater than `​0​`, such as due to being
blocked in `acquire`, will subsequently be unblocked.

### Preconditions

Both `update >= 0` and `update <= max() - counter` are `true`, where `counter`
is the value of the internal counter.

### Parameters

- **update** — the amount to increment the internal counter by

### Exceptions

May throw `std::system_error`.

### See also

- **acquire** — decrements the internal counter or blocks until it can (public
  member function)
- **try_acquire** — tries to decrement the internal counter without blocking
  (public member function)
- **try_acquire_for** — tries to decrement the internal counter, blocking for up
  to a duration time (public member function)
- **try_acquire_until** — tries to decrement the internal counter, blocking
  until a point in time (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/release*
