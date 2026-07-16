# std::queue<T,Container>::front

```cpp
reference front();
const_reference front() const;
```

Returns reference to the first element in the queue. This element will be the
first element to be removed on a call to `pop()`. Effectively calls `c.front()`.

### Parameters

(none)

### Return value

Reference to the first element.

### Complexity

Constant.

### See also

- **back** — access the last element (public member function)
- **pop** — removes the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/queue/front*
