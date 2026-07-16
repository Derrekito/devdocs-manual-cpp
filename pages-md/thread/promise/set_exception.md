# std::promise<R>::set_exception

```cpp
void set_exception( std::exception_ptr p );  // (since C++11)
```

Atomically stores the exception pointer `p` into the shared state and makes the
state ready.

The operation behaves as though `set_value`, `set_exception`,
`set_value_at_thread_exit`, and `set_exception_at_thread_exit` acquire a single
mutex associated with the promise object while updating the promise object.

An exception is thrown if there is no shared state or the shared state already
stores a value or exception.

Calls to this function do not introduce data races with calls to `get_future`
(therefore they need not synchronize with each other).

### Parameters

- **p** — exception pointer to store. The behavior is undefined if `p` is null

### Return value

(none)

### Exceptions

`std::future_error` on the following conditions:

- `*this` has no shared state. The error code is set to `no_state`.
- The shared state already stores a value or exception. The error code is set to
  `promise_already_satisfied`.

### Example

```cpp
#include <future>
#include <iostream>
#include <thread>

int main()
{
    std::promise<int> p;
    std::future<int> f = p.get_future();

    std::thread t([&p]
    {
        try
        {
            // code that may throw
            throw std::runtime_error("Example");
        }
        catch (...)
        {
            try
            {
                // store anything thrown in the promise
                p.set_exception(std::current_exception());
                // or throw a custom exception instead
                // p.set_exception(std::make_exception_ptr(MyException("mine")));
            }
            catch (...) {} // set_exception() may throw too
        }
    });

    try
    {
        std::cout << f.get();
    }
    catch (const std::exception& e)
    {
        std::cout << "Exception from the thread: " << e.what() << '\n';
    }
    t.join();
}
```

Output:

```text
Exception from the thread: Example
```

### See also

- **set_exception_at_thread_exit** — sets the result to indicate an exception
  while delivering the notification only at thread exit (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/set_exception*
