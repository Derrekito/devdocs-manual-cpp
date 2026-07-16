# std::multimap<Key,T,Compare,Allocator>::~multimap

```cpp
~multimap();
```

Destructs the `multimap`. The destructors of the elements are called and the
used storage is deallocated. Note, that if the elements are pointers, the
pointed-to objects are not destroyed.

### Complexity

Linear in the size of the `multimap`.

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/%7Emultimap*
