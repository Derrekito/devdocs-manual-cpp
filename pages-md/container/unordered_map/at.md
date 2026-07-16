# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::at

Returns a reference to the mapped value for the given key — and
**throws `std::out_of_range` if the key isn't present**, instead of
inserting one. That's the key difference from `operator[]`: `at`
never modifies the map, which is also why it has a `const` overload
usable on a `const unordered_map`.

```cpp skip
m.at(key);              // T&, throws std::out_of_range on miss
std::as_const(m).at(key);  // const T&, same throw behavior
```

### What you provide

- **key** — the key to look up.

### Guarantees and costs

- Average O(1); worst case linear in the size of the container (a
  degenerate hash puts everything in one bucket).
- Returns `T&` (or `const T&` from the const overload) to the existing
  element's mapped value — never inserts, never allocates.
- Throws `std::out_of_range` if no element has that key.

### Gotchas

- `at` is the only member-access form usable on a `const unordered_map`
  for a possibly-missing key; `operator[]` won't compile there because
  it's non-const (it must be able to insert).
- If a missing key is an expected, ordinary case rather than an error,
  prefer `find` and check for `end()` — throwing an exception for an
  expected outcome is needlessly expensive and reads as a bug to
  callers.
- The worst-case linear cost is a real risk with a poorly chosen or
  adversarial `Hash`, not just a theoretical bound — it's the same
  degradation `find` and `operator[]` share.

### Example

```cpp
#include <iostream>
#include <stdexcept>
#include <string>
#include <unordered_map>

int main()
{
    const std::unordered_map<std::string, int> ages{{"Ann", 30}, {"Bo", 25}};

    std::cout << "Ann is " << ages.at("Ann") << '\n';   // const map: fine

    try
    {
        std::cout << ages.at("Cy") << '\n';
    }
    catch (const std::out_of_range&)
    {
        std::cout << "Cy: no such key\n";
    }
}
```

```text
Ann is 30
Cy: no such key
```

### Reference

```cpp skip
T& at( const Key& key );  // (since C++11)
const T& at( const Key& key ) const;  // (since C++11)
```

Complexity: average O(1), worst case linear in `size()`.

### See also

- **operator[]** — access or insert, without bounds checking
- **find** — returns an iterator instead of throwing on miss
- **insert** — adds an element only if the key is absent

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/at*
