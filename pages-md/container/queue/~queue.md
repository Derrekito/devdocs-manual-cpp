# std::queue<T,Container>::~queue

```cpp
~queue();
```

Destructs the `queue`. The destructors of the elements are called and the used
storage is deallocated. Note, that if the elements are pointers, the pointed-to
objects are not destroyed.

### Complexity

Linear in the size of the `queue`.

---
*Source: https://en.cppreference.com/w/cpp/container/queue/%7Equeue*
