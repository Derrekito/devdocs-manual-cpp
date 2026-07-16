# std::jthread

```cpp
class jthread;  // (since C++20)
```

The class `jthread` represents a single thread of execution. It has the same
general behavior as `std::thread`, except that `jthread` automatically rejoins
on destruction, and can be cancelled/stopped in certain situations.

Threads begin execution immediately upon construction of the associated thread
object (pending any OS scheduling delays), starting at the top-level function
provided as a constructor argument. The return value of the top-level function
is ignored and if it terminates by throwing an exception, `std::terminate` is
called. The top-level function may communicate its return value or an exception
to the caller via `std::promise` or by modifying shared variables (which may
require synchronization, see `std::mutex` and `std::atomic`).

Unlike `std::thread`, the `jthread` logically holds an internal private member
of type `std::stop_source`, which maintains a shared stop-state. The `jthread`
constructor accepts a function that takes a `std::stop_token` as its first
argument, which will be passed in by the `jthread` from its internal
`std::stop_source`. This allows the function to check if stop has been requested
during its execution, and return if it has.

`std::jthread` objects may also be in the state that does not represent any
thread (after default construction, move from, `detach`, or `join`), and a
thread of execution may be not associated with any `jthread` objects (after
`detach`).

No two `std::jthread` objects may represent the same thread of execution;
`std::jthread` is not CopyConstructible or CopyAssignable, although it is
MoveConstructible and MoveAssignable.

### Member types

- **`id`** — `std::thread::id`
- **`native_handle_type`(not always present)** —
  `std::thread::native_handle_type`

### Member functions

- **(constructor)** — constructs new `jthread` object (public member function)
- **(destructor)** — if the thread is joinable, then a stop is requested and the
  thread joins (public member function)
- **operator=** — moves the `jthread` object (public member function)

**Observers**

- **joinable** — checks whether the thread is joinable, i.e. potentially running
  in parallel context (public member function)
- **get_id** — returns the *id* of the thread (public member function)
- **native_handle** — returns the underlying implementation-defined thread
  handle (public member function)
- **hardware_concurrency [static]** — returns the number of concurrent threads
  supported by the implementation (public static member function)

**Operations**

- **join** — waits for the thread to finish its execution (public member
  function)
- **detach** — permits the thread to execute independently from the thread
  handle (public member function)
- **swap** — swaps two jthread objects (public member function)

**Stop token handling**

- **get_stop_source** — returns a `stop_source` object associated with the
  shared stop state of the thread (public member function)
- **get_stop_token** — returns a `stop_token` associated with the shared stop
  state of the thread (public member function)
- **request_stop** — requests execution stop via the shared stop state of the
  thread (public member function)

### Non-member functions

- **swap(std::jthread) (C++20)** — specializes the `std::swap` algorithm
  (function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_jthread` | 201911L | (C++20) | Stop token and joining thread

### See also

- **thread (C++11)** — manages a separate thread (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread*
