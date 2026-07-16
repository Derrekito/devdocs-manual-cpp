# std::reverse_iterator<Iter>::operator++,+,+=,--,-,-=

```cpp
reverse_iterator& operator++();  // (until C++17)
constexpr reverse_iterator& operator++();  // (since C++17)
reverse_iterator& operator--();  // (until C++17)
constexpr reverse_iterator& operator--();  // (since C++17)
reverse_iterator operator++( int );  // (until C++17)
constexpr reverse_iterator operator++( int );  // (since C++17)
reverse_iterator operator--( int );  // (until C++17)
constexpr reverse_iterator operator--( int );  // (since C++17)
reverse_iterator operator+( difference_type n ) const;  // (until C++17)
constexpr reverse_iterator operator+( difference_type n ) const;  // (since C++17)
reverse_iterator operator-( difference_type n ) const;  // (until C++17)
constexpr reverse_iterator operator-( difference_type n ) const;  // (since C++17)
reverse_iterator& operator+=( difference_type n );  // (until C++17)
constexpr reverse_iterator& operator+=( difference_type n );  // (since C++17)
reverse_iterator& operator-=( difference_type n );  // (until C++17)
constexpr reverse_iterator& operator-=( difference_type n );  // (since C++17)
```

Increments or decrements the iterator. Inverse operations are applied to the
underlying operator because of the reverse order.

1,2) Pre-increments or pre-decrements by one respectively.

3,4) Post-increments or post-decrements by one respectively.

5,6) Returns an iterator which is advanced by `n` or `-n` positions
   respectively.

7,8) Advances the iterator by `n` or `-n` positions respectively.

### Parameters

- **n** — position relative to current location

### Return value

1,2) `*this`

3,4) A copy of `*this` that was made before the change.

5,6) `reverse_iterator(base()-n)` or `reverse_iterator(base()+n)` respectively.

7,8) `*this`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

int main()
{
    std::vector v{0, 1, 2, 3, 4};
    auto rv = std::reverse_iterator{v.rbegin()};
    std::cout << *(++rv) << ' '; // 3
    std::cout << *(--rv) << ' '; // 4
    std::cout << *(rv + 3) << ' '; // 1
    rv += 3;
    std::cout << rv[0] << ' '; // 1
    rv -= 3;
    std::cout << rv[0] << '\n'; // 4

    std::list l{5, 6, 7, 8};
    auto rl = std::reverse_iterator{l.rbegin()};
    std::cout << *(++rl) << ' '; // OK: 3
    std::cout << *(--rl) << '\n'; // OK: 4
    // The following statements raise compilation error because the
    // underlying iterator does not model the random access iterator:
//  *(rl + 3) = 13;
//  rl += 3;
//  rl -= 3;
}
```

Output:

```text
3 4 1 1 4
7 8
```

### See also

- **operator+** — advances the iterator (function template)
- **operator-** — computes the distance between two iterator adaptors (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/operator_arith*
