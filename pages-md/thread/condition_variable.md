# std::condition_variable

`std::condition_variable` (C++11) blocks one or more threads until
another thread changes a shared condition and notifies it. The recipe
is always the same three parts: a **mutex** protecting the shared
state, a **predicate** (a plain `bool` check of that state), and a
**notify** call from whichever thread changes the state. Always wait
with the predicate overload — `cv.wait(lock, []{ return ready; })` —
rather than a bare `wait`: the variable can wake up even when nothing
notified it (a *spurious wakeup*), and the predicate form re-checks and
loops until the condition genuinely holds.

```cpp skip
// waiting thread:
std::unique_lock<std::mutex> lk(m);
cv.wait(lk, []{ return ready; });  // immune to spurious wakeups

// notifying thread:
{
    std::lock_guard<std::mutex> lk(m);
    ready = true;                    // modify shared state under the lock
}
cv.notify_one();                     // wake one waiter (notify_all for all)
```

### What you provide

- **lock** — a locked `std::unique_lock<std::mutex>` on the mutex
  guarding the shared state. `wait` atomically unlocks it while
  sleeping and re-locks it before returning.
- **pred** — a callable taking no arguments and returning `bool`:
  "is the condition satisfied?" `wait` calls it in a loop, sleeping
  again whenever it returns `false`.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) / (destructor) | build / destroy the condition variable |
| `notify_one()` | wakes one waiting thread |
| `notify_all()` | wakes all waiting threads |
| `wait(lock)` | unlocks, sleeps until notified (or spuriously), re-locks |
| `wait(lock, pred)` | as above, looped until `pred()` is true |
| `wait_for(lock, dur[, pred])` | as `wait`, with a timeout duration |
| `wait_until(lock, tp[, pred])` | as `wait`, with a timeout time point |
| `native_handle()` | implementation-defined native handle |

### Guarantees and costs

- Works only with `std::unique_lock<std::mutex>`, which is what allows
  the most efficient implementation on most platforms; use
  `condition_variable_any` to wait with other lock types (e.g.
  `shared_lock`).
- The shared variable must be modified while the mutex is held, even
  if it's itself `atomic` — otherwise the notification can race the
  modification and a waiter can miss it.
- `notify_one`/`notify_all` may be called with or without holding the
  mutex; calling them after unlocking avoids waking a thread only to
  have it immediately block again on the mutex.
- `wait`, `wait_for`, `wait_until`, `notify_one`, and `notify_all` may
  all be called concurrently from different threads.
- Not CopyConstructible, MoveConstructible, CopyAssignable, or
  MoveAssignable.

### Gotchas

- Skipping the predicate and using bare `wait(lk)` is a bug magnet:
  spurious wakeups mean the thread can resume even though nothing
  actually changed, so the "waited, therefore true" assumption is
  false. Always pass a predicate, or manually loop
  `while (!condition) cv.wait(lk);`.
- Notifying without holding (or having held) the mutex while changing
  the state is a lost-wakeup bug: a waiter can check the predicate,
  find it false, and start waiting *after* the notify already fired,
  and then sleep forever.
- `wait` requires the lock to already be locked on entry — passing an
  unlocked `unique_lock` is undefined behavior.

### Example

```cpp
#include <condition_variable>
#include <iostream>
#include <mutex>
#include <thread>

std::mutex m;
std::condition_variable cv;
bool ready = false;
int result = 0;

void worker()
{
    std::unique_lock<std::mutex> lk(m);
    cv.wait(lk, []{ return ready; });
    result = 42;
}

int main()
{
    std::thread t(worker);

    {
        std::lock_guard<std::mutex> lk(m);
        ready = true;
    }
    cv.notify_one();

    t.join();
    std::cout << result << '\n';
}
```

```text
42
```

### Reference

```cpp skip
class condition_variable;  // (since C++11)
```

Formally: `condition_variable` is a StandardLayoutType.

### See also

- **condition_variable_any** — condition variable usable with any BasicLockable
- **unique_lock** — the lock type `condition_variable::wait` requires
- **mutex** — the basic mutual-exclusion primitive
- **notify_all_at_thread_exit** — arranges a notification when a thread exits

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable*
