# std::shared_future

```cpp
template< class T > class shared_future;  // (1) (since C++11)
template< class T > class shared_future<T&>;  // (2) (since C++11)
template<> class shared_future<void>;  // (3) (since C++11)
```

The class template `std::shared_future` provides a mechanism to access the
result of asynchronous operations, similar to `std::future`, except that
multiple threads are allowed to wait for the same shared state. Unlike
`std::future`, which is only moveable (so only one instance can refer to any
particular asynchronous result), `std::shared_future` is copyable and multiple
shared future objects may refer to the same shared state.

Access to the same shared state from multiple threads is safe if each thread
does it through its own copy of a `shared_future` object.

### Member functions

- **(constructor)** — constructs the future object (public member function)
- **(destructor)** — destructs the future object (public member function)
- **operator=** — assigns the contents (public member function)

**Getting the result**

- **get** — returns the result (public member function)

**State**

- **valid** — checks if the future has a shared state (public member function)
- **wait** — waits for the result to become available (public member function)
- **wait_for** — waits for the result, returns if it is not available for the
  specified timeout duration (public member function)
- **wait_until** — waits for the result, returns if it is not available until
  specified time point has been reached (public member function)

### Example

A `shared_future` may be used to signal multiple threads simultaneously, similar
to `std::condition_variable::notify_all()`.

```cpp
#include <chrono>
#include <future>
#include <iostream>

int main()
{
    std::promise<void> ready_promise, t1_ready_promise, t2_ready_promise;
    std::shared_future<void> ready_future(ready_promise.get_future());

    std::chrono::time_point<std::chrono::high_resolution_clock> start;

    auto fun1 = [&, ready_future]() -> std::chrono::duration<double, std::milli>
    {
        t1_ready_promise.set_value();
        ready_future.wait(); // waits for the signal from main()
        return std::chrono::high_resolution_clock::now() - start;
    };

    auto fun2 = [&, ready_future]() -> std::chrono::duration<double, std::milli>
    {
        t2_ready_promise.set_value();
        ready_future.wait(); // waits for the signal from main()
        return std::chrono::high_resolution_clock::now() - start;
    };

    auto fut1 = t1_ready_promise.get_future();
    auto fut2 = t2_ready_promise.get_future();

    auto result1 = std::async(std::launch::async, fun1);
    auto result2 = std::async(std::launch::async, fun2);

    // wait for the threads to become ready
    fut1.wait();
    fut2.wait();

    // the threads are ready, start the clock
    start = std::chrono::high_resolution_clock::now();

    // signal the threads to go
    ready_promise.set_value();

    std::cout << "Thread 1 received the signal "
              << result1.get().count() << " ms after start\n"
              << "Thread 2 received the signal "
              << result2.get().count() << " ms after start\n";
}
```

Possible output:

```text
Thread 1 received the signal 0.072 ms after start
Thread 2 received the signal 0.041 ms after start
```

### See also

- **async (C++11)** — runs a function asynchronously (potentially in a new
  thread) and returns a `std::future` that will hold the result (function
  template)
- **future (C++11)** — waits for a value that is set asynchronously (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future*
