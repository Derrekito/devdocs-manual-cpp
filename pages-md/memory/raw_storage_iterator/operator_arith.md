# std::raw_storage_iterator<OutputIt,T>::operator++, operator++(int)

```cpp
raw_storage_iterator& operator++();
raw_storage_iterator operator++( int );
```

Advances the iterator.

1) Pre-increment. Returns the updated iterator.

2) Post-increment. Returns the old value of the iterator.

### Parameters

(none)

### Return value

1) `*this`

2) The old value of the iterator.

---
*Source: https://en.cppreference.com/w/cpp/memory/raw_storage_iterator/operator_arith*
