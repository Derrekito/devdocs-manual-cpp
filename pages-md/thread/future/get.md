# std::future<T>::get

```cpp
T get();  // (1) (member only of generic future template)(since C++11)
T& get();  // (2) (member only of future<T&> template specialization)(since C++11)
void get();  // (3) (member only of future<void> template specialization)(since C++11)
```

The `get` member function waits until the `future` has a valid result and
(depending on which template is used) retrieves it. It effectively calls
`wait()` in order to wait for the result.

The generic template and two template specializations each contain a single
version of `get`. The three versions of `get` differ only in the return type.

The behavior is undefined if `valid()` is `false` before the call to this
function.

Any shared state is released. `valid()` is `false` after a call to this member
function.

### Parameters

(none)

### Return value

1) The value `v` stored in the shared state, as `std::move(v)`.

2) The reference stored as value in the shared state.

3) Nothing.

### Exceptions

If an exception was stored in the shared state referenced by the future (e.g.
via a call to `std::promise::set_exception()`) then that exception will be
thrown.

### Notes

The implementations are encouraged to detect the case when `valid()` is `false`
before the call and throw a `std::future_error` with an error condition of
`std::future_errc::no_state`.

### Example

```cpp
#include <chrono>
#include <future>
#include <iostream>
#include <string>
#include <thread>

std::string time()
{
    static auto start = std::chrono::steady_clock::now();
    std::chrono::duration<double> d = std::chrono::steady_clock::now() - start;
    return "[" + std::to_string(d.count()) + "s]";
}

int main()
{
    using namespace std::chrono_literals;
    {
        std::cout << time() << " launching thread\n";
        std::future<int> f = std::async(std::launch::async, []
        {
            std::this_thread::sleep_for(1s);
            return 7;
        });
        std::cout << time() << " waiting for the future, f.valid() == "
                  << f.valid() << '\n';
        int n = f.get();
        std::cout << time() << " future.get() returned with " << n << ". f.valid() = "
                  << f.valid() << '\n';
    }

    {
        std::cout << time() << " launching thread\n";
        std::future<int> f = std::async(std::launch::async, []
        {
            std::this_thread::sleep_for(1s);
            return true ? throw std::runtime_error("7") : 7;
        });
        std::cout << time() << " waiting for the future, f.valid() == "
                  << f.valid() << '\n';
        try
        {
            int n = f.get();
            std::cout << time() << " future.get() returned with " << n
                      << " f.valid() = " << f.valid() << '\n';
        }
        catch (const std::exception& e)
        {
            std::cout << time() << " caught exception " << e.what()
                      << ", f.valid() == " << f.valid() << '\n';
        }
    }
}
```

Possible output:

```text
[0.000004s] launching thread
[0.000461s] waiting for the future, f.valid() == 1
[1.001156s] future.get() returned with 7. f.valid() = 0
[1.001192s] launching thread
[1.001275s] waiting for the future, f.valid() == 1
[2.002356s] caught exception 7, f.valid() == 0
```

### See also

- **valid** — checks if the future has a shared state (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/future/get*
