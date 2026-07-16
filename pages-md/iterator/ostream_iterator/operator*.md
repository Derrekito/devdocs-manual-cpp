# std::ostream_iterator<T,CharT,Traits>::operator*

```cpp
ostream_iterator& operator*();
```

Does nothing, this member function is provided to satisfy the requirements of
LegacyOutputIterator.

It returns the iterator itself, which makes it possible to use code such as
`*iter = value` to output (insert) the value into the underlying stream.

### Parameters

(none)

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostream_iterator/operator**
