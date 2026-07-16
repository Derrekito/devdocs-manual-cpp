# std::condition_variable_any::~condition_variable_any

```cpp
~condition_variable_any();  // (since C++11)
```

Destroys the object of type `std::condition_variable_any`.

It is only safe to invoke the destructor if all threads have been notified. It
is not required that they have exited their respective wait functions: some
threads may still be waiting to reacquire the associated lock, or may be waiting
to be scheduled to run after reacquiring it.

The programmer must ensure that no threads attempt to wait on `*this` once the
destructor has been started, especially when the waiting threads are calling the
wait functions in a loop or are using the overloads of the wait functions that
take a predicate.

### See also

**C documentation for `cnd_destroy`**

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable_any/%7Econdition_variable_any*
