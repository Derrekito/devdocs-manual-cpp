# std::unordered_set<Key,Hash,KeyEqual,Allocator>::~unordered_set

```cpp
~unordered_set();  // (since C++11)
```

Destructs the `unordered_set`. The destructors of the elements are called and
the used storage is deallocated. Note, that if the elements are pointers, the
pointed-to objects are not destroyed.

### Complexity

Linear in the size of the `unordered_set`.

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/%7Eunordered_set*
