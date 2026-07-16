# std::map<Key,T,Compare,Allocator>::~map

```cpp
~map();
```

Destructs the `map`. The destructors of the elements are called and the used
storage is deallocated. Note, that if the elements are pointers, the pointed-to
objects are not destroyed.

### Complexity

Linear in the size of the `map`.

---
*Source: https://en.cppreference.com/w/cpp/container/map/%7Emap*
