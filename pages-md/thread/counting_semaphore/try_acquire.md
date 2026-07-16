# std::counting_semaphore<LeastMaxValue>::try_acquire

```cpp
bool try_acquire() noexcept;  // (since C++20)
```

Tries to atomically decrement the internal counter by `1` if it is greater than
`​0​`; no blocking occurs regardless.

### Return value

`true` if it decremented the internal counter, otherwise `false`.

### Notes

Implementations are allowed to fail to decrement the counter even if it was
greater than `​0​` - i.e., they are allowed to spuriously fail and return
`false`.

### See also

- **release** — increments the internal counter and unblocks acquirers (public
  member function)
- **acquire** — decrements the internal counter or blocks until it can (public
  member function)
- **try_acquire_for** — tries to decrement the internal counter, blocking for up
  to a duration time (public member function)
- **try_acquire_until** — tries to decrement the internal counter, blocking
  until a point in time (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/try_acquire*
