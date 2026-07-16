# std::cv_status

```cpp
enum class cv_status {
    no_timeout,
    timeout
};  // (since C++11)
```

The scoped enumeration `std::cv_status` describes whether a timed wait returned
because of timeout or not.

`std::cv_status` is used by the `wait_for` and `wait_until` member functions of
`std::condition_variable` and `std::condition_variable_any`.

### Member constants

- **`no_timeout`** — the condition variable was awakened with `notify_all`,
  `notify_one`, or spuriously
- **`timeout`** — the condition variable was awakened by timeout expiration

### See also

- **wait_for** — blocks the current thread until the condition variable is
  awakened or after the specified timeout duration (public member function of
  `std::condition_variable`)
- **wait_for** — blocks the current thread until the condition variable is
  awakened or after the specified timeout duration (public member function of
  `std::condition_variable_any`)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function of `std::condition_variable`)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function of `std::condition_variable_any`)

---
*Source: https://en.cppreference.com/w/cpp/thread/cv_status*
