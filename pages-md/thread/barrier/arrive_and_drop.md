# std::barrier<CompletionFunction>::arrive_and_drop

```cpp
void arrive_and_drop();  // (since C++20)
```

Decrements the initial expected count for all subsequent phases by one, and then
decrements the expected count for the current phase by one.

This function is executed atomically. The call to this function strongly
happens-before the start of the phase completion step for the current phase.

The behavior is undefined if the expected count for the current phase is zero.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws `std::system_error` with an error code allowed for mutex types on error.

### Notes

This function can cause the completion step for the current phase to start.

If the current expected count is zero before calling this function, the initial
expected count for all subsequent phases is also zero, which means the `barrier`
cannot be reused.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier/arrive_and_drop*
