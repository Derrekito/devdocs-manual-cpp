# std::deque<T,Allocator>::~deque

```cpp
~deque();
```

Destructs the `deque`. The destructors of the elements are called and the used
storage is deallocated. Note, that if the elements are pointers, the pointed-to
objects are not destroyed.

### Complexity

Linear in the size of the `deque`.

---
*Source: https://en.cppreference.com/w/cpp/container/deque/%7Edeque*
