# std::multimap<Key,T,Compare,Allocator>::find

```cpp
iterator find( const Key& key );  // (1)
const_iterator find( const Key& key ) const;  // (2)
template< class K >
iterator find( const K& x );  // (3) (since C++14)
template< class K >
const_iterator find( const K& x ) const;  // (4) (since C++14)
```

1,2) Finds an element with key equivalent to `key`. If there are several
   elements with `key` in the container, any of them may be returned.

3,4) Finds an element with key that compares *equivalent* to the value `x`. This
   overload participates in overload resolution only if the qualified-id
   `Compare::is_transparent` is valid and denotes a type. It allows calling this
   function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the element to search for
- **x** — a value of any type that can be transparently compared with a key

### Return value

Iterator to an element with key equivalent to `key`. If no such element is
found, past-the-end (see `end()`) iterator is returned.

### Complexity

Logarithmic in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_associative_lookup` | 201304L | (C++14) | Heterogeneous
      comparison lookup in associative containers; overloads (3,4)

### Example

```cpp
#include <iostream>
#include <map>

struct FatKey   { int x; int data[1000]; };
struct LightKey { int x; };
// Note: as detailed above, the container must use std::less<> (or other
// transparent Comparator) to access these overloads.
// This includes standard overloads, such as between std::string and std::string_view.
bool operator<(const FatKey& fk, const LightKey& lk) { return fk.x < lk.x; }
bool operator<(const LightKey& lk, const FatKey& fk) { return lk.x < fk.x; }
bool operator<(const FatKey& fk1, const FatKey& fk2) { return fk1.x < fk2.x; }

int main()
{
    // Simple comparison demo.
    std::multimap<int,char> example = {{1,'a'}, {2,'b'}};

    if (auto search = example.find(2); search != example.end())
        std::cout << "Found " << search->first << ' ' << search->second << '\n';
    else
        std::cout << "Not found\n";

    // Transparent comparison demo.
    std::multimap<FatKey, char, std::less<>> example2 = {{{1, {}}, 'a'}, {{2, {}}, 'b'}};

    LightKey lk = {2};
    if (auto search = example2.find(lk); search != example2.end())
        std::cout << "Found " << search->first.x << ' ' << search->second << '\n';
    else
        std::cout << "Not found\n";

    // Obtaining const iterators.
    // Compiler decides whether to return iterator of (non) const type by way of
    // accessing map; to prevent modification on purpose, one of easiest choices
    // is to access map by const reference.
    const auto& example2ref = example2;
    if (auto search = example2ref.find(lk); search != example2.end())
    {
        std::cout << "Found " << search->first.x << ' ' << search->second << '\n';
    //  search->second = 'c'; // error: assignment of member
                              // 'std::pair<const FatKey, char>::second'
                              // in read-only object
    }
}
```

Output:

```text
Found 2 b
Found 2 b
Found 2 b
```

### See also

- **count** — returns the number of elements matching specific key (public
  member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/find*
