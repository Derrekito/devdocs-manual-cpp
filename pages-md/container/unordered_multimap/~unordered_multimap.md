# std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::~unordered_multimap

```cpp
~unordered_multimap();  // (since C++11)
```

Destructs the `unordered_multimap`. The destructors of the elements are called
and the used storage is deallocated. Note, that if the elements are pointers,
the pointed-to objects are not destroyed.

### Complexity

Linear in the size of the `unordered_multimap`.

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/%7Eunordered_multimap*
