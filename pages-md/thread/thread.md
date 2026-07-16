# std::thread

```cpp
class thread;  // (since C++11)
```

The class `thread` represents a single thread of execution. Threads allow
multiple functions to execute concurrently.

Threads begin execution immediately upon construction of the associated thread
object (pending any OS scheduling delays), starting at the top-level function
provided as a constructor argument. The return value of the top-level function
is ignored and if it terminates by throwing an exception, `std::terminate` is
called. The top-level function may communicate its return value or an exception
to the caller via `std::promise` or by modifying shared variables (which may
require synchronization, see `std::mutex` and `std::atomic`).

`std::thread` objects may also be in the state that does not represent any
thread (after default construction, move from, `detach`, or `join`), and a
thread of execution may not be associated with any `thread` objects (after
`detach`).

No two `std::thread` objects may represent the same thread of execution;
`std::thread` is not CopyConstructible or CopyAssignable, although it is
MoveConstructible and MoveAssignable.

### Member types

- **`native_handle_type`(not always present)** — *implementation-defined*

### Member classes

- **id** — represents the *id* of a thread (public member class)

### Member functions

- **(constructor)** — constructs new `thread` object (public member function)
- **(destructor)** — destructs the thread object, underlying thread must be
  joined or detached (public member function)
- **operator=** — moves the thread object (public member function)

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
- **swap** — swaps two thread objects (public member function)

### Non-member functions

- **std::swap(std::thread) (C++11)** — specializes the `std::swap` algorithm
  (function)

### See also

- **jthread (C++20)** — `std::thread` with support for auto-joining and
  cancellation (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/thread*
