# std::queue<T,Container>::back

```cpp
reference back();
const_reference back() const;
```

Returns reference to the last element in the queue. This is the most recently
pushed element. Effectively calls `c.back()`.

### Parameters

(none)

### Return value

Reference to the last element.

### Complexity

Constant.

### See also

- **front** — access the first element (public member function)
- **push** — inserts element at the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/queue/back*
