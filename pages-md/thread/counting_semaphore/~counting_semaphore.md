# std::counting_semaphore<LeastMaxValue>::~counting_semaphore

```cpp
~counting_semaphore();  // (since C++20)
```

Destroys the `counting_semaphore` object.

The behavior is undefined if any thread is concurrently calling any other member
function on this semaphore. This includes threads blocked in `acquire()`,
`try_acquire_for()`, or `try_acquire_until()`.

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/%7Ecounting_semaphore*
