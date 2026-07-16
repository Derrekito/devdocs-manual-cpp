# std::promise<R>::set_exception_at_thread_exit

```cpp
void set_exception_at_thread_exit( std::exception_ptr p );  // (since C++11)
```

Stores the exception pointer `p` into the shared state without making the state
ready immediately. The state is made ready when the current thread exits, after
all variables with thread-local storage duration have been destroyed.

The operation behaves as though `set_value`, `set_exception`,
`set_value_at_thread_exit`, and `set_exception_at_thread_exit` acquire a single
mutex associated with the promise object while updating the promise object.

An exception is thrown if there is no shared state or the shared state already
stores a value or exception.

Calls to this function do not introduce data races with calls to `get_future`
(therefore they need not synchronize with each other).

### Parameters

- **p** — exception pointer to store. The behavior is undefined if `p` is
  `nullptr`

### Return value

(none)

### Exceptions

`std::future_error` on the following conditions:

- `*this` has no shared state. The error code is set to `no_state`.
- The shared state already stores a value or exception. The error code is set to
  `promise_already_satisfied`.

### Example

### See also

- **set_exception** — sets the result to indicate an exception (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/set_exception_at_thread_exit*
