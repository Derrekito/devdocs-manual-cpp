# std::barrier<CompletionFunction>::arrive

```cpp
[[nodiscard]] arrival_token arrive( std::ptrdiff_t n = 1 );  // (since C++20)
```

Constructs an `arrival_token` object associated with the phase synchronization
point for the current phase. Then, decrements the expected count by `n`.

This function executes atomically. The call to this function strongly
happens-before the start of the phase completion step for the current phase.

The behavior is undefined if `n` is less than or equal to 0 or greater than the
expected count for the current barrier phase.

### Parameters

- **n** — the value by which the expected count is decreased

### Return value

The constructed `arrival_token` object.

### Exceptions

Throws `std::system_error` with an error code allowed for mutex types on error.

### Notes

This function can cause the completion step for the current phase to start.

### Example

### See also

- **wait** — blocks at the phase synchronization point until its phase
  completion step is run (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier/arrive*
