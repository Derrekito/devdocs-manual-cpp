# std::move_iterator<Iter>::operator++,+,+=,--,-,-=

```cpp
move_iterator& operator++();  // (until C++17)
constexpr move_iterator& operator++();  // (since C++17)
move_iterator& operator--();  // (until C++17)
constexpr move_iterator& operator--();  // (since C++17)
move_iterator operator++( int );  // (until C++17)
constexpr move_iterator operator++( int );  // (since C++17) (until C++20)
constexpr auto operator++( int );  // (since C++20)
move_iterator operator--( int );  // (until C++17)
constexpr move_iterator operator--( int );  // (since C++17)
move_iterator operator+( difference_type n ) const;  // (until C++17)
constexpr move_iterator operator+( difference_type n ) const;  // (since C++17)
move_iterator operator-( difference_type n ) const;  // (until C++17)
constexpr move_iterator operator-( difference_type n ) const;  // (since C++17)
move_iterator& operator+=( difference_type n );  // (until C++17)
constexpr move_iterator& operator+=( difference_type n );  // (since C++17)
move_iterator& operator-=( difference_type n );  // (until C++17)
constexpr move_iterator& operator-=( difference_type n );  // (since C++17)
```

Increments or decrements the iterator.

1,2) Pre-increments or pre-decrements by one respectively.

3,4) Post-increments or post-decrements by one respectively.

5,6) Returns an iterator which is advanced by `n` or `-n` positions
   respectively.

7,8) Advances the iterator by `n` or `-n` positions respectively.

### Parameters

- **n** — position relative to current location

### Return value

1,2) `*this`

3,4) A copy of `*this` that was made before the change, however, if `Iter` does
   not model `forward_iterator`, the post-increment operator does not return
   such copy and the return type is `void`(since C++20).

5,6) `move_iterator(base()+n)` or `move_iterator(base()-n)` respectively.

7,8) `*this`

### Example

### See also

- **operator+ (C++11)** — advances the iterator (function template)
- **operator- (C++11)** — computes the distance between two iterator adaptors
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator_arith*
