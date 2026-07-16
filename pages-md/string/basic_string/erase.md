# std::basic_string<CharT,Traits,Allocator>::erase

Removes characters and returns something different depending on which
form you call: the index/count form returns `*this`; both iterator
forms return an iterator to what now follows the erased region (or
`end()` if nothing does).

```cpp skip
s.erase(index, count);  // removes count chars from index; returns *this
s.erase(pos);           // removes one char at pos; returns next iterator
s.erase(first, last);   // removes [first, last); returns iterator at last
```

### What you provide

- **index** — first character to remove (default `0`).
- **count** — number of characters to remove (default `npos`, meaning
  "to the end"). Values that overshoot are clamped, not an error.
- **position** — iterator to the single character to remove; must be
  dereferenceable on `*this`, or behavior is undefined.
- **first, last** — a valid range `[first, last)` on `*this`; an
  invalid iterator or range is undefined behavior.

### Guarantees and costs

- Index/count form removes `std::min(count, size() - index)`
  characters; throws `std::out_of_range` if `index > size()`.
- Iterator forms throw nothing, but require valid iterators — the
  library trusts you here instead of checking.
- Strong exception safety: if any overload throws, `*this` is
  unchanged.
- `constexpr` since C++20.

### Gotchas

- The single-iterator overload returns the iterator *after* the
  erased character, not the one you passed in — that iterator is now
  invalid.
- Any erase invalidates iterators/references from the erasure point
  onward; reuse the returned iterator instead of the old one to keep
  iterating safely.
- `s.erase(s.find(ch))` is a common idiom to trim everything from the
  first occurrence of `ch` to the end (index/count form, `count`
  defaults to `npos`).

### Example

```cpp c++20
#include <algorithm>
#include <iostream>
#include <string>

int main()
{
    std::string s = "This Is An Example";

    s.erase(7, 3);  // index/count: erases " An"
    std::cout << s << '\n';

    s.erase(std::find(s.begin(), s.end(), ' '));  // single iterator
    std::cout << s << '\n';

    s.erase(s.find(' '));  // index/count: trims to the end
    std::cout << s << '\n';
}
```

```text
This Is Example
ThisIs Example
ThisIs
```

### Reference

```cpp skip
basic_string& erase( size_type index = 0, size_type count = npos );  // (until C++20)
constexpr basic_string&
    erase( size_type index = 0, size_type count = npos );  // (since C++20)
iterator erase( iterator position );  // (until C++11)
iterator erase( const_iterator position );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator position );  // (since C++20)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first,
                const_iterator last );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator first,
                          const_iterator last );  // (since C++20)
```

(LWG 27) originally the range overload returned the iterator following
`last`'s character rather than the one pointing at it; corrected
retroactively. (LWG 847) strong exception safety was added
retroactively as well.

### See also

- **clear** — removes all characters
- **substr** — returns a copy of a range without modifying the string
- **find** — locates a character or substring to erase from
- **insert** — adds characters at a position

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/erase*
