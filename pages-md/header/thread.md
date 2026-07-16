# Standard library header <thread> (C++11)

`<thread>` is the thread support library: it defines `std::thread`, a
handle to an OS-level thread of execution, plus the free functions in
`std::this_thread` that act on the calling thread itself (yield,
sleep, get its id). Include it whenever you're launching or managing
threads directly rather than through a higher-level facility like
`<future>`'s `async`.

`std::jthread` (C++20) is a `thread` with two differences: it joins
itself automatically in its destructor instead of requiring an
explicit `join()`/`detach()`, and it carries a `stop_source` so
cooperative cancellation (`request_stop()`, `stop_token`) works without
extra bookkeeping. Prefer it over plain `thread` in new code.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support

### Namespaces

- **`this_thread`** — functions that act on the calling thread

### Classes

- **thread** (C++11) — manages a separate thread of execution
- **jthread** (C++20) — a `thread` that auto-joins and supports
  cooperative cancellation
- **std::hash<thread::id>** — hash support for `thread::id`

### Functions

- **std::swap(thread)** (C++11) — specializes `std::swap`
- **operator==** and the relational operators — compares two
  `thread::id` objects (`operator<=>` added in C++20; the other
  relational overloads were removed then, synthesized from it)
- **operator<<** — writes a `thread::id` to an output stream
- **this_thread::yield** (C++11) — suggests the scheduler reschedule
  other threads
- **this_thread::get_id** (C++11) — the id of the calling thread
- **this_thread::sleep_for** (C++11) — blocks the calling thread for a
  duration
- **this_thread::sleep_until** (C++11) — blocks the calling thread
  until a time point

---
*Source: https://en.cppreference.com/w/cpp/header/thread*
