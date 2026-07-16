# std::barrier<CompletionFunction>::~barrier

```cpp
~barrier();  // (since C++20)
```

Destroys the `barrier`.

### Notes

The behavior is undefined if any thread is concurrently calling a member
function of the `barrier`.

---
*Source: https://en.cppreference.com/w/cpp/thread/barrier/%7Ebarrier*
