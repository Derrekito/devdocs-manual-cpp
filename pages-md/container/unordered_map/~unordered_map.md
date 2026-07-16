# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::~unordered_map

```cpp
~unordered_map();  // (since C++11)
```

Destructs the `unordered_map`. The destructors of the elements are called and
the used storage is deallocated. Note, that if the elements are pointers, the
pointed-to objects are not destroyed.

### Complexity

Linear in the size of the `unordered_map`.

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/%7Eunordered_map*
