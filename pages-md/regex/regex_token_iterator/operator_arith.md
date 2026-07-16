# std::regex_token_iterator<BidirIt,CharT,Traits>::operator++, operator++(int)

```cpp
regex_token_iterator& operator++();  // (since C++11)
regex_token_iterator operator++( int );  // (since C++11)
```

Advances the iterator on the next sub match.

If `*this` is a suffix iterator, sets `*this` to an end-of-sequence iterator.

Otherwise, if `N + 1 < subs.size()`, increments N and sets result to the address
of the current match.

Otherwise, sets `N` to `​0​` and increments `position`. If `position` is not an
end-of-sequence iterator the operator sets result to the address of the current
match.

Otherwise, if any of the values stored in subs is equal to `-1` and
`prev->suffix().length()` is not `​0​` the operator sets `*this` to a suffix
iterator that points to the range [`prev->suffix().first`,
`prev->suffix().second`).

Otherwise, sets `*this` to an end-of-sequence iterator.

The behavior is undefined if the iterator is end-of-sequence iterator.

### Parameters

(none)

### Return value

1) `*this`

2) The previous value of the iterator.

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_token_iterator/operator_arith*
