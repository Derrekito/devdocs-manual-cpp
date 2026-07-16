# std::istream_iterator<T,CharT,Traits,Distance>::~istream_iterator

```cpp
~istream_iterator();  // (until C++11)
~istream_iterator() = default;  // (since C++11)
```

Destroys the iterator, including the cached value.

If `std::is_trivially_destructible<T>::value` is `true`, then this destructor is
a trivial destructor.
*(since C++11)*

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator/%7Eistream_iterator*
