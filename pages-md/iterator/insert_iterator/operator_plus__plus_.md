# std::insert_iterator<Container>::operator++

```cpp
insert_iterator& operator++();  // (until C++20)
constexpr insert_iterator& operator++();  // (since C++20)
insert_iterator& operator++( int );  // (until C++20)
constexpr insert_iterator& operator++( int );  // (since C++20)
```

Does nothing. These operator overloads are provided to satisfy the requirements
of LegacyOutputIterator. They make it possible for the expressions
`*iter++=value` and `*++iter=value` to be used to output (insert) a value into
the underlying container.

### Parameters

(none)

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/iterator/insert_iterator/operator%2B%2B*
