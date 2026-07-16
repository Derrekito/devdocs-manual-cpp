# std::istream_iterator<T,CharT,Traits,Distance>::operator++, operator++(int)

```cpp
istream_iterator& operator++();  // (1)
istream_iterator operator++( int );  // (2)
```

Reads a value from the underlying stream (using its `operator>>`) and stores it
into the iterator object. If the read fails (the underlying stream's `fail()`
returns `true`), the iterator becomes the end-of-stream iterator.

The behavior is undefined if the iterator is end-of-stream iterator.

### Parameters

(none)

### Return value

1) `*this`

2) An `istream_iterator` that holds an unchanged value.

### Exceptions

May throw implementation-defined exceptions.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 260 | C++98 | the return type of operator++(int) was `istream_iterator&` |
      corrected to `istream_iterator`
  LWG 788 | C++98 | the underlying stream's operator void* was used to determine
      whether the read fails, but it was removed by the resolution of LWG issue
      468 | uses `fail()` instead
  LWG 838 | C++98 | it was unclear whether end-of-stream iterators can be
      incremented | the behavior is undefined

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator/operator_arith*
