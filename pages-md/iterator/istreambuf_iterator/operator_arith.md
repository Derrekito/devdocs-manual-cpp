# std::istreambuf_iterator<CharT,Traits>::operator++, operator++(int)

```cpp
istreambuf_iterator& operator++();  // (1)
/* proxy */ operator++( int );  // (2)
```

Advances the iterator by calling `sbuf_->sbumpc()` where `sbuf_` is the stored
pointer to the stream buffer.

The behavior is undefined if the iterator is end-of-stream iterator.

### Parameters

(none)

### Return value

1) `*this`

2) A `proxy` object holding the current character obtained via `operator*()` and
   the `sbuf_` pointer. Dereferencing a `proxy` object with `operator*` yields
   the stored character.

The name `proxy` is for exposition only.

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/iterator/istreambuf_iterator/operator_arith*
