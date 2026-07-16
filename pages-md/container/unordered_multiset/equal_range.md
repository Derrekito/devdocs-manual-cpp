# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::equal_range

```cpp
std::pair<iterator, iterator> equal_range( const Key& key );  // (1) (since C++11)
std::pair<const_iterator, const_iterator> equal_range( const Key& key ) const;  // (2) (since C++11)
template< class K >
std::pair<iterator, iterator> equal_range( const K& x );  // (3) (since C++20)
template< class K >
std::pair<const_iterator, const_iterator> equal_range( const K& x ) const;  // (4) (since C++20)
```

1,2) Returns a range containing all elements with key `key` in the container.
   The range is defined by two iterators, the first pointing to the first
   element of the wanted range and the second pointing past the last element of
   the range.

3,4) Returns a range containing all elements in the container with key
   equivalent to `x`. This overload participates in overload resolution only if
   `Hash::is_transparent` and `KeyEqual::is_transparent` are valid and each
   denotes a type. This assumes that such `Hash` is callable with both `K` and
   `Key` type, and that the `KeyEqual` is transparent, which, together, allows
   calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value to compare the elements to
- **x** — a value of any type that can be transparently compared with a key

### Return value

`std::pair` containing a pair of iterators defining the wanted range. If there
are no such elements, past-the-end (see `end()`) iterators are returned as both
elements of the pair.

### Complexity

Average case linear in the number of elements with the key `key`, worst case
linear in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_unordered_lookup` | 201811L | (C++20) | Heterogeneous
      comparison lookup in unordered associative containers, overloads (3,4)

### Example

Estimates the characters frequency for given string.

```cpp
#include <iostream>
#include <iterator>
#include <string>
#include <unordered_set>

int main()
{
    std::string sentence{"cppreference.com"};
    std::cout << "The sentence: " << sentence << '\n';

    std::unordered_multiset<char> sequence;
    for (char x : sentence)
        sequence.insert(x);

    std::cout << "The sequence: { ";
    for (char x : sequence)
        std::cout << x << ' ';

    std::cout << "}\n" "Symbol:Frequency: ";
    for (auto it = sequence.begin(); it != sequence.end();)
    {
        if (auto [first, last] = sequence.equal_range(*it); first != last)
        {
            std::cout << *first << ":" << std::distance(first, last) << "  ";
            it = last;
        }
        else
            ++it;
    }
}
```

Possible output:

```text
The sentence: cppreference.com
The sequence: { m o c c c p p r r e e e e f n . }
Symbol:Frequency: m:1  o:1  c:3  p:2  r:2  e:4  f:1  n:1  .:1
```

### See also

- **find** — finds element with specific key (public member function)
- **contains (C++20)** — checks if the container contains element with specific
  key (public member function)
- **count** — returns the number of elements matching specific key (public
  member function)
- **equal_range** — returns range of elements matching a specific key (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/equal_range*
