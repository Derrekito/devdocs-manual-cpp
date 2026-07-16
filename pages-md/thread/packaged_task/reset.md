# std::packaged_task<R(Args...)>::reset

```cpp
void reset();  // (since C++11)
```

Resets the state abandoning the results of previous executions. New shared state
is constructed.

Equivalent to `*this = packaged_task(std::move(f))`, where `f` is the stored
task.

### Parameters

(none)

### Return value

(none)

### Exceptions

- `std::future_error` if `*this` has no shared state. The error condition is set
  to `no_state`.
- `std::bad_alloc` if there was not enough memory for a new shared state.
- Any exception thrown by the move constructor of the new `packaged_task`.

### Example

```cpp
#include <cmath>
#include <future>
#include <iostream>
#include <thread>

int main()
{
    std::packaged_task<int(int,int)> task([](int a, int b)
    {
        return std::pow(a, b);
    });
    std::future<int> result = task.get_future();
    task(2, 9);
    std::cout << "2^9 = " << result.get() << '\n';

    task.reset();
    result = task.get_future();
    std::thread task_td(std::move(task), 2, 10);
    task_td.join();
    std::cout << "2^10 = " << result.get() << '\n';
}
```

Output:

```text
2^9 = 512
2^10 = 1024
```

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/reset*
