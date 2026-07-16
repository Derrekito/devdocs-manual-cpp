# std::packaged_task

```cpp
template< class > class packaged_task; //not defined  // (1) (since C++11)
template< class R, class ...ArgTypes >
class packaged_task<R(ArgTypes...)>;  // (2) (since C++11)
```

The class template `std::packaged_task` wraps any Callable target (function,
lambda expression, bind expression, or another function object) so that it can
be invoked asynchronously. Its return value or exception thrown is stored in a
shared state which can be accessed through `std::future` objects.

Just like `std::function`, `std::packaged_task` is a polymorphic,
allocator-aware container: the stored callable target may be allocated on heap
or with a provided allocator.
*(until C++17)*

### Member functions

- **(constructor)** — constructs the task object (public member function)
- **(destructor)** — destructs the task object (public member function)
- **operator=** — moves the task object (public member function)
- **valid** — checks if the task object has a valid function (public member
  function)
- **swap** — swaps two task objects (public member function)

**Getting the result**

- **get_future** — returns a `std::future` associated with the promised result
  (public member function)

**Execution**

- **operator()** — executes the function (public member function)
- **make_ready_at_thread_exit** — executes the function ensuring that the result
  is ready only once the current thread exits (public member function)
- **reset** — resets the state abandoning any stored results of previous
  executions (public member function)

### Non-member functions

- **std::swap(std::packaged_task) (C++11)** — specializes the `std::swap`
  algorithm (function template)

### Helper classes

- **std::uses_allocator<std::packaged_task> (C++11) (until C++17)** —
  specializes the `std::uses_allocator` type trait (class template
  specialization)

### Deduction guides(since C++17)

### Example

```cpp
#include <cmath>
#include <functional>
#include <future>
#include <iostream>
#include <thread>

// unique function to avoid disambiguating the std::pow overload set
int f(int x, int y) { return std::pow(x, y); }

void task_lambda()
{
    std::packaged_task<int(int, int)> task([](int a, int b)
    {
        return std::pow(a, b);
    });
    std::future<int> result = task.get_future();

    task(2, 9);

    std::cout << "task_lambda:\t" << result.get() << '\n';
}

void task_bind()
{
    std::packaged_task<int()> task(std::bind(f, 2, 11));
    std::future<int> result = task.get_future();

    task();

    std::cout << "task_bind:\t" << result.get() << '\n';
}

void task_thread()
{
    std::packaged_task<int(int, int)> task(f);
    std::future<int> result = task.get_future();

    std::thread task_td(std::move(task), 2, 10);
    task_td.join();

    std::cout << "task_thread:\t" << result.get() << '\n';
}

int main()
{
    task_lambda();
    task_bind();
    task_thread();
}
```

Output:

```text
task_lambda: 512
task_bind:   2048
task_thread: 1024
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3117 | C++17 | deduction guides for `packaged_task` were missing | added

### See also

- **future (C++11)** — waits for a value that is set asynchronously (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task*
