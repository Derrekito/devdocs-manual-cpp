# std::thread

`std::thread` (C++11) represents a single OS thread of execution: the
callable passed to its constructor starts running immediately (modulo
scheduling delay). The one rule that matters: every `thread` that is
still **joinable** — not yet joined, detached, default-constructed, or
moved-from — must have `join()` or `detach()` called on it before its
destructor runs, or the destructor calls `std::terminate` and the
program aborts. Where available, prefer `std::jthread` (C++20): it
joins automatically in its destructor and supports cooperative
cancellation.

```cpp skip
std::thread t;                 // no associated thread (empty state)
std::thread t(f, args...);     // starts running f(args...) now
t.joinable();                  // true if join()/detach() is still owed
t.join();                      // block until it finishes; now not joinable
t.detach();                    // let it run independently; now not joinable
```

### What you provide

- **f, args...** — a callable and the arguments to invoke it with, run
  as `f(args...)` on the new thread. Its return value is discarded; if
  it exits via an exception, `std::terminate` is called. Get results
  back out through `std::promise`/`std::future`, or a shared variable
  guarded by a `mutex` or `atomic`.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | starts a new thread running `f(args...)` |
| (destructor) | destroys the object; `std::terminate` if still joinable |
| `operator=` | move-assigns the thread; not copy-assignable |
| `joinable()` | true if there's a thread that still needs join/detach |
| `get_id()` | this thread's `std::thread::id` |
| `native_handle()` | implementation-defined OS thread handle |
| `hardware_concurrency()` [static] | usable hardware threads, 0 if unknown |
| `join()` | blocks until the thread finishes; then not joinable |
| `detach()` | thread runs independently; then not joinable |
| `swap(other)` | exchanges which OS thread each object represents |

Non-member `std::swap` is also provided.

### Guarantees and costs

- No two `thread` objects ever represent the same thread of execution:
  `thread` is move-only (MoveConstructible/MoveAssignable), never
  copyable.
- A `thread` holds "no thread" after default construction, being moved
  from, `join()`, or `detach()` — that's exactly the state
  `joinable()` reports.
- `join()` throws `std::system_error` if `joinable()` is false (e.g.
  calling it twice), or if a thread tries to join itself.
- Destroying a still-joinable thread calls `std::terminate` — this is
  unconditional, not an exception that can be caught.

### Gotchas

- The classic bug: an exception or early `return` skips the `join()`/
  `detach()` call, so the *next* stack unwind destroys a joinable
  thread and terminates the program. Either join unconditionally
  (e.g. in a scope that always runs to its end) or use `std::jthread`.
- `detach()` cuts the thread loose entirely — it may still be running
  after `main()` returns, reading destroyed globals. Only detach work
  that owns everything it touches.
- Arguments are copied/moved into the thread's internal storage; to
  pass by reference, wrap the argument in `std::ref`/`std::cref`,
  otherwise it decays to a copy.

### Example

```cpp
#include <iostream>
#include <thread>
#include <vector>

int main()
{
    std::vector<int> results(3);

    std::thread t1([&]{ results[0] = 10; });
    std::thread t2([&]{ results[1] = 20; });
    std::thread t3([&]{ results[2] = 30; });

    t1.join();
    t2.join();
    t3.join();

    for (int r : results)
        std::cout << r << ' ';
    std::cout << '\n';
}
```

```text
10 20 30
```

### Reference

```cpp skip
class thread;  // (since C++11)
```

Formally: `thread::id` is a member class identifying a thread;
`native_handle_type` is present only when the implementation offers a
native handle. `thread` satisfies none of CopyConstructible or
CopyAssignable, but does satisfy MoveConstructible and MoveAssignable.

### See also

- **jthread** (C++20) — `thread` that auto-joins and supports cancellation
- **mutex** — protects data shared between threads
- **condition_variable** — blocks a thread until notified of a condition
- **async** — runs a callable, possibly on another thread, returning a `future`

---
*Source: https://en.cppreference.com/w/cpp/thread/thread*
