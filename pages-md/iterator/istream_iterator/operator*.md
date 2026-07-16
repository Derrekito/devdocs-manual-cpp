# std::istream_iterator<T,CharT,Traits,Distance>::operator*, operator->

```cpp
const T& operator*() const;  // (1)
const T* operator->() const;  // (2)
```

Returns a pointer or a reference to the current element.

The behavior is undefined if the iterator is end-of-stream iterator.

### Parameters

(none)

### Return value

A pointer or a reference to the current element.

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator/operator**
