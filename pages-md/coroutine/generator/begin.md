# std::generator<Ref,V,Allocator>::begin

```cpp
/*iterator*/ begin();  // (since C++23)
```

Pushes `coroutine_` into `*active_` stack, then evaluates `coroutine_.resume()`.

Before invocation of `begin()` the `coroutine_` must refer to a coroutine
suspended at its initial suspend point.

### Return value

An iterator whose `coroutine_` member refers to the same coroutine as
`coroutine_` does.

### Notes

It is an undefined behavior to call `begin()` more than once on the same
`generator` object.

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/begin*
