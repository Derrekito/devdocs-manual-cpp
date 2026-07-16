# std::vector<T,Allocator>::emplace_back

Constructs a new element in place at the end, forwarding `args` straight
to `T`'s constructor — unlike `push_back`, no separate object is built
and then copied or moved in. Amortized O(1); the same invalidation rule
as `push_back` applies (reallocation invalidates everything, otherwise
only `end()`). Since C++17 it returns a reference to the new element;
before that, it returns nothing.

```cpp skip
v.emplace_back(args...);              // (until C++17) returns void
auto& r = v.emplace_back(args...);    // (since C++17) returns the element
```

### What you provide

- **args** — arguments forwarded to `T`'s constructor as
  `std::forward<Args>(args)...`. `T` must be MoveInsertable (needed in
  case of reallocation) and EmplaceConstructible from `args`.

### Guarantees and costs

- Amortized constant time.
- Reallocation (new `size()` > old `capacity()`): all iterators,
  pointers, and references are invalidated, `end()` included. Otherwise,
  only `end()` is invalidated.
- Strong exception guarantee, with the same move-constructor caveat as
  `push_back`: if `T`'s move constructor isn't `noexcept` and `T` isn't
  CopyInsertable, the throwing move constructor is used, and a throw
  from it leaves the effects unspecified.
- *(until C++17)* return type is `void`. *(since C++17)* returns a
  reference to the newly constructed element.

### Gotchas

- `emplace_back` only avoids the copy/move of *the new* element — a
  reallocation still move-constructs every existing element into the new
  storage, same as `push_back`.
- Before C++17 you can't chain off the call (`emplace_back(...).foo()`)
  since the return type is `void`.

### Example

`emplace_back` forwards its arguments straight to `President`'s
constructor, avoiding the extra move that `push_back` needs when handed
an already-built temporary.

```cpp
#include <vector>
#include <iostream>
#include <string>

struct President
{
    std::string name;
    int year;

    President(std::string p_name, int p_year)
        : name(std::move(p_name)), year(p_year)
    {
        std::cout << "constructed\n";
    }

    President(President&& other)
        : name(std::move(other.name)), year(other.year)
    {
        std::cout << "moved\n";
    }
};

int main()
{
    std::vector<President> elections;
    std::cout << "emplace_back:\n";
    auto& ref = elections.emplace_back("Nelson Mandela", 1994);
    std::cout << "returned reference: " << (ref.year == 1994) << '\n';

    std::vector<President> reElections;
    std::cout << "push_back:\n";
    reElections.push_back(President("F. D. Roosevelt", 1936));
}
```

```text
emplace_back:
constructed
returned reference: 1
push_back:
constructed
moved
```

### Reference

```cpp skip
template< class... Args >
void emplace_back( Args&&... args );  // (since C++11) (until C++17)
template< class... Args >
reference emplace_back( Args&&... args );  // (since C++17) (until C++20)
template< class... Args >
constexpr reference emplace_back( Args&&... args );  // (since C++20)
```

The element is constructed through `std::allocator_traits::construct`,
typically placement-new at the location the container provides.

### See also

- **push_back** — appends a copy or move of an existing object
- **emplace** (C++11) — constructs an element in place at an arbitrary
  position

---
*Source: https://en.cppreference.com/w/cpp/container/vector/emplace_back*
