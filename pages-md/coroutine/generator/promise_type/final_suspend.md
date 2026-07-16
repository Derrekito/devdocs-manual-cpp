# std::generator<Ref,V,Allocator>::promise_type::final_suspend

```cpp
auto final_suspend() noexcept;  // (since C++23)
```

Let `x` be some generator object.

`final_suspend` does the following:

1. Pops the coroutine handle from the top of `*active_`.
1. If `*x.active_` is not empty, resumes execution of the coroutine referred to
   by `x.active_->top()`. If it is empty, control flow returns to the current
   coroutine caller or resumer.

A handle referring to the coroutine whose promise object is `*this` must be at
the top of `*x.active_` of `x`. This function must be called by the coroutine
upon reaching its final suspend point, otherwise the behavior is undefined.

### Parameters

(none)

### Return value

An awaitable object of unspecified type whose member functions are configured to
suspend the calling coroutine.

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/final_suspend*
