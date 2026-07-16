# std::set<Key,Compare,Allocator>::~set

```cpp
~set();
```

Destructs the `set`. The destructors of the elements are called and the used
storage is deallocated. Note, that if the elements are pointers, the pointed-to
objects are not destroyed.

### Complexity

Linear in the size of the `set`.

---
*Source: https://en.cppreference.com/w/cpp/container/set/%7Eset*
