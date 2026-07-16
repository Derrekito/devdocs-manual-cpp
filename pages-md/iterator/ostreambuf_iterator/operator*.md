# std::ostreambuf_iterator<CharT,Traits>::operator*

```cpp
ostreambuf_iterator& operator*();
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
*Source: https://en.cppreference.com/w/cpp/iterator/ostreambuf_iterator/operator**
