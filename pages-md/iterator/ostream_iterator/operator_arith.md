# std::ostream_iterator<T,CharT,Traits>::operator++

```cpp
ostream_iterator& operator++();
ostream_iterator& operator++( int );
```

Does nothing. These operator overloads are provided to satisfy the requirements
of LegacyOutputIterator. They make it possible for the expressions
`*iter++=value` and `*++iter=value` to be used to output (insert) a value into
the underlying stream.

### Parameters

(none)

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostream_iterator/operator_arith*
