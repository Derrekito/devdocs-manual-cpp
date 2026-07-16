# std::barrier<CompletionFunction>::arrive_and_wait

```cpp
void arrive_and_wait();  // (since C++20)
```

Atomically decrements the expected count by 1, then blocks at the
synchronization point for the current phase until the phase completion step of
the current phase is run. Equivalent to `wait(arrive());`.

The behavior is undefined if the expected count for the current phase is zero.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws `std::system_error` with an error code allowed for mutex types on error.

### Notes

If the current expected count is decremented to zero in the call to this
function, the phase completion step is run and this function does not block.

If the current expected count is zero before calling this function, the initial
expected count for all subsequent phases is also zero, which means the `barrier`
cannot be reused.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier/arrive_and_wait*
