# std::latch::~latch

```cpp
~latch();  // (since C++20)
```

Destroys the `latch`.

### Notes

The behavior is undefined if any thread is concurrently calling a member
function of the `latch`.

---
*Source: https://en.cppreference.com/w/cpp/thread/latch/%7Elatch*
