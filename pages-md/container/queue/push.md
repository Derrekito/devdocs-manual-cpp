# std::queue<T,Container>::push

```cpp
void push( const value_type& value );
void push( value_type&& value );  // (since C++11)
```

Pushes the given element `value` to the end of the queue.

1) Effectively calls `c.push_back(value)`.

2) Effectively calls `c.push_back(std::move(value))`.

### Parameters

- **value** — the value of the element to push

### Return value

(none)

### Complexity

Equal to the complexity of `Container::push_back`.

### Example

### See also

- **emplace (C++11)** — constructs element in-place at the end (public member
  function)
- **pop** — removes the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/queue/push*
