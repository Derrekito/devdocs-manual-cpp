# std::packaged_task<R(Args...)>::operator()

```cpp
void operator()( ArgTypes... args );  // (since C++11)
```

Calls the stored task as if by `INVOKE<R>(f, args...)`, where `f` is the stored
task. The return value of the task or any exceptions thrown are stored in the
shared state. The shared state is made ready and any threads waiting for this
are unblocked.

### Parameters

- **args** — the parameters to pass on invocation of the stored task

### Return value

(none)

### Exceptions

`std::future_error` on the following error conditions:

- The stored task has already been invoked. The error category is set to
  `promise_already_satisfied`.
- `*this` has no shared state. The error category is set to `no_state`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2142 | C++11 | a successful call to operator() synchronized with a call to
      any member function of a `std::future` or `std::shared_future` that share
      their shared state with `*this` | no additional synchronization guarantees
      other than what is already provided by shared state

### See also

- **make_ready_at_thread_exit** — executes the function ensuring that the result
  is ready only once the current thread exits (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/operator()*
