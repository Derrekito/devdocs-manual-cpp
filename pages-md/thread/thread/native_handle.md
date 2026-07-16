# std::thread::native_handle

```cpp
native_handle_type native_handle();  // (since C++11) (not always present)
```

Returns the implementation defined underlying thread handle.

### Parameters

(none)

### Return value

Implementation defined handle type representing the thread.

### Exceptions

May throw implementation-defined exceptions.

### Example

Uses `native_handle` to enable realtime scheduling of C++ threads on a POSIX
system.

```cpp
#include <chrono>
#include <cstring>
#include <iostream>
#include <mutex>
#include <pthread.h>
#include <thread>

std::mutex iomutex;
void f(int num)
{
    std::this_thread::sleep_for(std::chrono::seconds(1));

    sched_param sch;
    int policy;
    pthread_getschedparam(pthread_self(), &policy, &sch);
    std::lock_guard<std::mutex> lk(iomutex);
    std::cout << "Thread " << num << " is executing at priority "
              << sch.sched_priority << '\n';
}

int main()
{
    std::thread t1(f, 1), t2(f, 2);

    sched_param sch;
    int policy;
    pthread_getschedparam(t1.native_handle(), &policy, &sch);
    sch.sched_priority = 20;
    if (pthread_setschedparam(t1.native_handle(), SCHED_FIFO, &sch))
        std::cout << "Failed to setschedparam: " << std::strerror(errno) << '\n';

    t1.join(); t2.join();
}
```

Output:

```text
Thread 2 is executing at priority 0
Thread 1 is executing at priority 20
```

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/native_handle*
