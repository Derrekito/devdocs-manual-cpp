# std::recursive_mutex

```cpp
class recursive_mutex;  // (since C++11)
```

The `recursive_mutex` class is a synchronization primitive that can be used to
protect shared data from being simultaneously accessed by multiple threads.

`recursive_mutex` offers exclusive, recursive ownership semantics:

- A calling thread *owns* a `recursive_mutex` for a period of time that starts
  when it successfully calls either `lock` or `try_lock`. During this period,
  the thread may make additional calls to `lock` or `try_lock`. The period of
  ownership ends when the thread makes a matching number of calls to `unlock`.
- When a thread owns a `recursive_mutex`, all other threads will block (for
  calls to `lock`) or receive a `false` return value (for `try_lock`) if they
  attempt to claim ownership of the `recursive_mutex`.
- The maximum number of times that a `recursive_mutex` may be locked is
  unspecified, but after that number is reached, calls to `lock` will throw
  `std::system_error` and calls to `try_lock` will return `false`.

The behavior of a program is undefined if a `recursive_mutex` is destroyed while
still owned by some thread. The `recursive_mutex` class satisfies all
requirements of Mutex and StandardLayoutType.

### Member types

- **`native_handle_type`(not always present)** — *implementation-defined*

### Member functions

- **(constructor)** — constructs the mutex (public member function)
- **(destructor)** — destroys the mutex (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Locking**

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **unlock** — unlocks the mutex (public member function)

**Native handle**

- **native_handle** — returns the underlying implementation-defined native
  handle object (public member function)

### Example

One use case for `recursive_mutex` is protecting shared state in a class whose
member functions may call each other.

```cpp
#include <iostream>
#include <mutex>
#include <thread>

class X
{
    std::recursive_mutex m;
    std::string shared;
public:
    void fun1()
    {
        std::lock_guard<std::recursive_mutex> lk(m);
        shared = "fun1";
        std::cout << "in fun1, shared variable is now " << shared << '\n';
    }
    void fun2()
    {
        std::lock_guard<std::recursive_mutex> lk(m);
        shared = "fun2";
        std::cout << "in fun2, shared variable is now " << shared << '\n';
        fun1(); // recursive lock becomes useful here
        std::cout << "back in fun2, shared variable is " << shared << '\n';
    }
};

int main()
{
    X x;
    std::thread t1(&X::fun1, &x);
    std::thread t2(&X::fun2, &x);
    t1.join();
    t2.join();
}
```

Possible output:

```text
in fun1, shared variable is now fun1
in fun2, shared variable is now fun2
in fun1, shared variable is now fun1
back in fun2, shared variable is fun1
```

### See also

- **mutex (C++11)** — provides basic mutual exclusion facility (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/recursive_mutex*
