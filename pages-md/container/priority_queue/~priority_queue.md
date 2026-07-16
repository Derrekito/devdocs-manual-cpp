# std::priority_queue<T,Container,Compare>::~priority_queue

```cpp
~priority_queue();
```

Destructs the `priority_queue`. The destructors of the elements are called and
the used storage is deallocated. Note, that if the elements are pointers, the
pointed-to objects are not destroyed.

### Complexity

Linear in the size of the `priority_queue`.

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/%7Epriority_queue*
