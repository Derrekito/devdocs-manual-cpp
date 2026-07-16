# std::packaged_task<R(Args...)>::packaged_task

```cpp
packaged_task() noexcept;  // (1) (since C++11)
template< class F >
explicit packaged_task( F&& f );  // (2) (since C++11)
template< class F, class Allocator >
explicit packaged_task( std::allocator_arg_t, const Allocator& a, F&& f );  // (3) (since C++11) (until C++17)
packaged_task( const packaged_task& ) = delete;  // (4) (since C++11)
packaged_task( packaged_task&& rhs ) noexcept;  // (5) (since C++11)
```

Constructs a new `std::packaged_task` object.

1) Constructs a `std::packaged_task` object with no task and no shared state.
2,3) Constructs a `std::packaged_task` object with a shared state and a copy of
the task, initialized with `std::forward<F>(f)`. The allocator `a` is used to
allocate memory necessary to store the task.(until C++17)

- These constructors do not(until C++17)This constructor does not(since C++17)
  participate in overload resolution if std::decay<F>::type is the same type as
  std::packaged_task<R(ArgTypes...)>.
- The program is ill-formed if `INVOKE<R>(std::forward<F>(f),
  std::declval<Args>()...)` is ill-formed when treated as an unevaluated operand
  (i.e. `std::is_invocable_r_v<R, F, Args...>` is not `true`)(since C++17).
- The behavior is undefined if the invocation on a copy of `f` behaves different
  from that on `f`.

4) The copy constructor is deleted, `std::packaged_task` is move-only.

5) Constructs a `std::packaged_task` with the shared state and task formerly
   owned by `rhs`, leaving `rhs` with no shared state and a moved-from task.

### Parameters

- **f** — the callable target (function, member function, lambda expression,
  function object) to execute
- **a** — the allocator to use when storing the task
- **rhs** — the `std::packaged_task` to move from

### Exceptions

2) Any exceptions thrown by copy/move constructor of `f` and possibly
   `std::bad_alloc` if the allocation fails.

3) Any exceptions thrown by copy/move constructor of `f` and by the allocator's
   `allocate` function if memory allocation fails.

### Example

```cpp
#include <future>
#include <iostream>
#include <thread>

int fib(int n)
{
    if (n < 3)
        return 1;
    else
        return fib(n - 1) + fib(n - 2);
}

int main()
{
    std::packaged_task<int(int)> fib_task(&fib);

    std::cout << "Starting task\n";
    auto result = fib_task.get_future();
    std::thread t(std::move(fib_task), 42);

    std::cout << "Waiting for task to finish..." << std::endl;
    std::cout << result.get() << '\n';

    std::cout << "Task complete\n";
    t.join();
}
```

Output:

```text
Starting task
Waiting for task to finish...
267914296
Task complete
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2067 | C++11 | the parameter type of the copy constructor was
      `packaged_task&` | added const

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/packaged_task*
