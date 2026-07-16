# std::barrier

```cpp
template< class CompletionFunction = /* see below */ >
class barrier;  // (since C++20)
```

The class template `std::barrier` provides a thread-coordination mechanism that
blocks a group of threads of known size until all threads in that group have
reached the barrier. Unlike `std::latch`, barriers are reusable: once a group of
arriving threads are unblocked, the barrier can be reused. Unlike `std::latch`,
barriers execute a possibly empty callable before unblocking threads.

A barrier object's lifetime consists of one or more phases. Each phase defines a
*phase synchronization point* where waiting threads block. Threads can arrive at
the barrier, but defer waiting on the *phase synchronization point* by calling
`arrive`. Such threads can later block on the *phase synchronization point* by
calling `wait`.

A barrier *phase* consists of the following steps:

1. The *expected count* is decremented by each call to `arrive` or
   `arrive_and_drop`.
1. When the expected count reaches zero, the *phase completion step* is run,
   meaning that the `CompletionFunction` is invoked, and all threads blocked on
   the phase synchronization point are unblocked. The end of the completion step
   strongly happens-before all calls that were unblocked by the completion step
   return. Exactly once after the expected count reaches zero, a thread executes
   the completion step during its call to `arrive`, `arrive_and_drop`, or
   `wait`, except that it is implementation-defined whether the step executes if
   no thread calls `wait`.
1. When the completion step finishes, the expected count is reset to the value
   specified at construction less the number of calls to `arrive_and_drop`
   since, and the next *barrier phase* begins.

Concurrent invocations of the member functions of `barrier`, except for the
destructor, do not introduce data races.

### Template parameters

- **CompletionFunction** — a function object type

**-`CompletionFunction` must meet the requirements of MoveConstructible and Destructible. `std::is_nothrow_invocable_v<CompletionFunction&>` must be `true`.**

The default template argument of `CompletionFunction` is an unspecified function
object type that additionally meets the requirements of DefaultConstructible.
Calling an lvalue of it with no arguments has no effects.

Every barrier object behaves as if it holds an exposition-only non-static data
member `completion_` of type `CompletionFunction` and calls it by
`completion_()` on every phase completion step.

### Member types

- **`arrival_token`** — an unspecified object type meeting requirements of
  MoveConstructible, MoveAssignable and Destructible

### Member functions

- **(constructor)** — constructs a `barrier` (public member function)
- **(destructor)** — destroys the `barrier` (public member function)
- **operator= [deleted]** — `barrier` is not assignable (public member function)
- **arrive** — arrives at barrier and decrements the expected count (public
  member function)
- **wait** — blocks at the phase synchronization point until its phase
  completion step is run (public member function)
- **arrive_and_wait** — arrives at barrier and decrements the expected count by
  one, then blocks until current phase completes (public member function)
- **arrive_and_drop** — decrements both the initial expected count for
  subsequent phases and the expected count for current phase by one (public
  member function)

**Constants**

- **max [static]** — the maximum value of expected count supported by the
  implementation (public static member function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_barrier` | 201907L | (C++20) | `std::barrier`
  202302L | (C++20) (DR) | Relaxed guarantees for phase completion

### Example

```cpp
#include <barrier>
#include <iostream>
#include <string>
#include <syncstream>
#include <thread>
#include <vector>

int main()
{
    const auto workers = {"Anil", "Busara", "Carl"};

    auto on_completion = []() noexcept
    {
        // locking not needed here
        static auto phase =
            "... done\n"
            "Cleaning up...\n";
        std::cout << phase;
        phase = "... done\n";
    };

    std::barrier sync_point(std::ssize(workers), on_completion);

    auto work = [&](std::string name)
    {
        std::string product = "  " + name + " worked\n";
        std::osyncstream(std::cout) << product;  // ok, op<< call is atomic
        sync_point.arrive_and_wait();

        product = "  " + name + " cleaned\n";
        std::osyncstream(std::cout) << product;
        sync_point.arrive_and_wait();
    };

    std::cout << "Starting...\n";
    std::vector<std::jthread> threads;
    threads.reserve(std::size(workers));
    for (auto const& worker : workers)
        threads.emplace_back(work, worker);
}
```

Possible output:

```text
Starting...
  Anil worked
  Carl worked
  Busara worked
... done
Cleaning up...
  Busara cleaned
  Carl cleaned
  Anil cleaned
... done
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2588R3 | C++20 | old phase completion guarantees might prevent hardware
      acceleration | relaxed

### See also

- **latch (C++20)** — single-use thread barrier (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier*
