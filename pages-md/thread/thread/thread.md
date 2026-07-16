# std::thread::thread

```cpp
thread() noexcept;  // (1) (since C++11)
thread( thread&& other ) noexcept;  // (2) (since C++11)
template< class Function, class... Args >
explicit thread( Function&& f, Args&&... args );  // (3) (since C++11)
thread( const thread& ) = delete;  // (4) (since C++11)
```

Constructs a new `std::thread` object.

1) Creates a new `std::thread` object which does not represent a thread.

2) Move constructor. Constructs the `std::thread` object to represent the thread
   of execution that was represented by `other`. After this call `other` no
   longer represents a thread of execution.

3) Creates a new `std::thread` object and associates it with a thread of
   execution. The new thread of execution starts executing:
   `INVOKE(decay-copy(std::forward<Function>(f)),
   decay-copy(std::forward<Args>(args))...)` (until C++23)
   `std::invoke(auto(std::forward<Function>(f)),
   auto(std::forward<Args>(args))...)` (since C++23)

The calls of `decay-copy` are evaluated(until C++23)The values produced by
   `auto` are materialized(since C++23) in the current thread, so that any
   exceptions thrown during evaluation and copying/moving of the arguments are
   thrown in the current thread, without starting the new thread. The program is
   ill-formed if any construction or the invoke operation is invalid.

This constructor does not participate in overload resolution if
   `std::decay<Function>::type`(until
   C++20)`std::remove_cvref_t<Function>`(since C++20) is the same type as
   `std::thread`.

The completion of the invocation of the constructor *synchronizes-with* (as
   defined in `std::memory_order`) the beginning of the invocation of the copy
   of `f` on the new thread of execution.

4) The copy constructor is deleted; threads are not copyable. No two
   `std::thread` objects may represent the same thread of execution.

### Parameters

- **other** — another thread object to construct this thread object with
- **f** — Callable object to execute in the new thread
- **args...** — arguments to pass to the new function

### Postconditions

1) `get_id()` equal to `std::thread::id()` (i.e. `joinable()` is `false`).

2) `other.get_id()` equal to `std::thread::id()` and `get_id()` returns the
   value of `other.get_id()` prior to the start of construction.

3) `get_id()` not equal to `std::thread::id()` (i.e. `joinable()` is `true`).

### Exceptions

3) `std::system_error` if the thread could not be started. The exception may
   represent the error condition `std::errc::resource_unavailable_try_again` or
   another implementation-specific error condition.

### Notes

The arguments to the thread function are moved or copied by value. If a
reference argument needs to be passed to the thread function, it has to be
wrapped (e.g., with `std::ref` or `std::cref`).

Any return value from the function is ignored. If the function throws an
exception, `std::terminate` is called. In order to pass return values or
exceptions back to the calling thread, `std::promise` or `std::async` may be
used.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>
#include <utility>

void f1(int n)
{
    for (int i = 0; i < 5; ++i)
    {
        std::cout << "Thread 1 executing\n";
        ++n;
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
}

void f2(int& n)
{
    for (int i = 0; i < 5; ++i)
    {
        std::cout << "Thread 2 executing\n";
        ++n;
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
}

class foo
{
public:
    void bar()
    {
        for (int i = 0; i < 5; ++i)
        {
            std::cout << "Thread 3 executing\n";
            ++n;
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
        }
    }
    int n = 0;
};

class baz
{
public:
    void operator()()
    {
        for (int i = 0; i < 5; ++i)
        {
            std::cout << "Thread 4 executing\n";
            ++n;
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
        }
    }
    int n = 0;
};

int main()
{
    int n = 0;
    foo f;
    baz b;
    std::thread t1; // t1 is not a thread
    std::thread t2(f1, n + 1); // pass by value
    std::thread t3(f2, std::ref(n)); // pass by reference
    std::thread t4(std::move(t3)); // t4 is now running f2(). t3 is no longer a thread
    std::thread t5(&foo::bar, &f); // t5 runs foo::bar() on object f
    std::thread t6(b); // t6 runs baz::operator() on a copy of object b
    t2.join();
    t4.join();
    t5.join();
    t6.join();
    std::cout << "Final value of n is " << n << '\n';
    std::cout << "Final value of f.n (foo::n) is " << f.n << '\n';
    std::cout << "Final value of b.n (baz::n) is " << b.n << '\n';
}
```

Possible output:

```text
Thread 1 executing
Thread 2 executing
Thread 3 executing
Thread 4 executing
Thread 3 executing
Thread 1 executing
Thread 2 executing
Thread 4 executing
Thread 2 executing
Thread 3 executing
Thread 1 executing
Thread 4 executing
Thread 3 executing
Thread 2 executing
Thread 1 executing
Thread 4 executing
Thread 3 executing
Thread 1 executing
Thread 2 executing
Thread 4 executing
Final value of n is 5
Final value of f.n (foo::n) is 5
Final value of b.n (baz::n) is 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2097 | C++11 | overload (2) might be ambiguous with overload (3) |
      constrained overload (3)

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### See also

- **(constructor)** — constructs new `jthread` object (public member function of
  `std::jthread`)

**C documentation for `thrd_create`**

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/thread*
