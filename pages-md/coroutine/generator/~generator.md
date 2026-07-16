# std::generator<Ref,V,Allocator>::~generator

```cpp
~generator();  // (since C++23)
```

Destructs the generator object.

Given `coroutine_` as the underlying coroutine object, equivalent to:

```cpp
if (coroutine_)
    coroutine_.destroy();
```

Note, that destroying the root generator effectively destroys the entire stack
of yielded generators, because the ownership of recursively yielded generators
is held in awaitable objects in the coroutine frame of the yielding generator.

### Complexity

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/%7Egenerator*
