# std::regex_token_iterator<BidirIt,CharT,Traits>::operator==, operator!=

```cpp
bool operator==( const regex_token_iterator& other ) const;  // (1) (since C++11)
bool operator!=( const regex_token_iterator& other ) const;  // (2) (since C++11) (until C++20)
```

Checks whether `*this` and `other` are equivalent.

Two regex token iterators are equal if:

a) They are both end-of-sequence iterators.

b) They are both suffix iterators and the suffixes are equal.

c) Neither of them is end-of-sequence or suffix iterator and:

- `position == other.position`
- `N == other.N`
- `subs == other.subs`

1) Checks whether `*this` is *equal to* `other`.

2) Checks whether `*this` is *not equal to* `other`.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **other** — another regex token iterator to compare to

### Return value

1) `true` if `*this` is *equal to* `other`, `false` otherwise.

2) `true` if `*this` is *not equal to* `other`, `false` otherwise.

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_token_iterator/operator_cmp*
