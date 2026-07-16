# std::generator<Ref,V,Allocator>::promise_type::initial_suspend

```cpp
std::suspend_always initial_suspend() const noexcept;  // (since C++23)
```

Equivalent to `{ return std::suspend_always{}; }`, that is, `std::generator`
always starts lazily (in suspended state).

### Parameters

(none)

### Return value

The awaitable object.

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/initial_suspend*
