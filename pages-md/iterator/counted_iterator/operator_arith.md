# std::counted_iterator<I>::operator++,+,+=,--,-,-=

```cpp
constexpr counted_iterator& operator++();  // (1) (since C++20)
constexpr decltype(auto) operator++( int );  // (2) (since C++20)
constexpr counted_iterator operator++( int )
    requires std::forward_iterator<I>;  // (3) (since C++20)
constexpr counted_iterator& operator--()
    requires std::bidirectional_iterator<I>;  // (4) (since C++20)
constexpr counted_iterator operator--( int )
    requires std::bidirectional_iterator<I>;  // (5) (since C++20)
constexpr counted_iterator operator+( std::iter_difference_t<I> n ) const
    requires std::random_access_iterator<I>;  // (6) (since C++20)
constexpr counted_iterator& operator+=( std::iter_difference_t<I> n )
    requires std::random_access_iterator<I>;  // (7) (since C++20)
constexpr counted_iterator operator-( std::iter_difference_t<I> n ) const
    requires std::random_access_iterator<I>;  // (8) (since C++20)
constexpr counted_iterator& operator-=( std::iter_difference_t<I> n )
    requires std::random_access_iterator<I>;  // (9) (since C++20)
```

Increments or decrements the underlying iterator `current` and the distance to
the end `length`.

The behavior of these functions is undefined if the `length` would be set to a
minus value.

1) Pre-increments by one. Equivalent to `++current; --length; return *this;`.

2) Post-increments by one. Equivalent to `--length; try { return current++; }
   catch(...) { ++length; throw; }`.

3) Post-increments by one. Equivalent to `counted_iterator temp{*this}; ++*this;
   return temp;`.

4) Pre-decrements by one. Equivalent to `--current; ++length; return *this;`.

5) Post-decrements by one. Equivalent to `counted_iterator temp{*this}; --*this;
   return temp;`.

6) Returns an iterator adaptor which is advanced by `n`. Equivalent to `return
   counted_iterator(current + n, length - n);`.

7) Advances the iterator adaptor by `n`. Equivalent to `current += n; length -=
   n; return *this;`.

8) Returns an iterator adaptor which is advanced by `-n`. Equivalent to `return
   counted_iterator(current - n, length + n);`.

9) Advances the iterator adaptor by `-n`. Equivalent to `current -= n; length +=
   n; return *this;`.

### Parameters

- **n** — the number of positions to increment or decrement the iterator adaptor

### Return value

1) `*this`

2,3) A copy of `*this` that was made before the change.

4) `*this`

5) A copy of `*this` that was made before the change.

6) An iterator adaptor which is advanced by `n`.

7) `*this`

8) An iterator adaptor which is advanced by `-n`.

9) `*this`

### Example

```cpp
#include <cassert>
#include <initializer_list>
#include <iterator>

int main()
{
    const auto v = {1, 2, 3, 4, 5, 6};
    std::counted_iterator<std::initializer_list<int>::iterator> it1{v.begin(), 5};

    ++it1;              assert(*it1 == 2 && it1.count() == 4); // (1)
    auto it2 = it1++;   assert(*it2 == 2 && *it1 == 3);        // (3)
    --it1;              assert(*it1 == 2 && it1.count() == 4); // (4)
    auto it3 = it1--;   assert(*it3 == 2 && *it1 == 1);        // (5)
    auto it4 = it1 + 3; assert(*it4 == 4 && it4.count() == 2); // (6)
    auto it5 = it4 - 3; assert(*it5 == 1 && it5.count() == 5); // (8)
    it1 += 3;           assert(*it1 == 4 && it1.count() == 2); // (7)
    it1 -= 3;           assert(*it1 == 1 && it1.count() == 5); // (9)
}
```

### See also

- **operator+ (C++20)** — advances the iterator (function template)
- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/operator_arith*
