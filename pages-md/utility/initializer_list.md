# std::initializer_list

`std::initializer_list<T>` (C++11) is a lightweight view over a
temporary array the compiler builds for you: writing `{a, b, c}` where
one is expected constructs a `const T` array behind the scenes and
hands you an `initializer_list` that points into it. The list itself
is typically just a pointer and a length — copying an
`initializer_list` copies that pointer and length, never the backing
array — and every element it exposes is `const T`, so you can't modify
elements through it.

```cpp skip
void f(std::initializer_list<int> l);

f({1, 2, 3});                    // braced-init-list as a function argument
auto l = {1, 2, 3};              // special rule: auto deduces initializer_list<int>
for (int x : {1, 2, 3}) { }      // braced-init-list bound to a range-for

l.size(); l.begin(); l.end();    // T*/const T* pair under the hood
```

### Guarantees and costs

- An `initializer_list` is automatically constructed when a
  braced-init-list is used to list-initialize an object whose
  constructor takes an `initializer_list` parameter, is used as the
  right operand of assignment or as a call argument to a function
  taking one, or is bound to `auto` (including in a range-for).
- May be implemented as a pointer and a length, or as a pair of
  pointers — either way, `size()`, `begin()`, and `end()` are O(1).
- Copying an `initializer_list` copies the view (pointer/length), not
  the backing array; the array's lifetime is tied to the temporary
  that created it, not to any copy of the list.
- `iterator`/`const_iterator` are both `const T*`; `reference` and
  `const_reference` are both `const T&` — there is no mutable access,
  by design.
- Declaring an explicit or partial specialization of
  `std::initializer_list` is ill-formed.

### Gotchas

- The backing array is a temporary. Once the full expression that
  created it ends, an `initializer_list` that still points to it is
  dangling — don't store one as a class member or return one from a
  function.
- Elements are always `const`; algorithms that need to modify elements
  in place don't work directly on an `initializer_list` (copy into a
  `vector` first).
- `templated_fn({1, 2, 3})` fails to deduce a template parameter — a
  braced-init-list is not an expression and has no type on its own.
  Give the parameter a concrete `initializer_list<T>` type, or spell
  out the template argument explicitly.

### Example

```cpp
#include <initializer_list>
#include <iostream>
#include <vector>

template<class T>
struct S
{
    std::vector<T> v;

    S(std::initializer_list<T> l) : v(l)
    {
        std::cout << "constructed with a " << l.size() << "-element list\n";
    }

    void append(std::initializer_list<T> l)
    {
        v.insert(v.end(), l.begin(), l.end());
    }
};

int main()
{
    S<int> s = {1, 2, 3, 4, 5};
    s.append({6, 7, 8});

    std::cout << "The vector now has " << s.v.size() << " ints:\n";
    for (std::size_t i = 0; i < s.v.size(); ++i)
    {
        if (i != 0)
            std::cout << ' ';
        std::cout << s.v[i];
    }
    std::cout << '\n';

    auto al = {10, 11, 12};          // special rule for auto
    std::cout << "al.size() = " << al.size() << '\n';
}
```

```text
constructed with a 5-element list
The vector now has 8 ints:
1 2 3 4 5 6 7 8
al.size() = 3
```

### Reference

```cpp skip
template< class T >
class initializer_list;  // (since C++11)
```

Member types: `value_type` = `T`; `reference`/`const_reference` =
`const T&`; `size_type` = `std::size_t`; `iterator`/`const_iterator` =
`const T*`.

**Member functions**: (constructor) — creates an empty list;
`size` (Capacity); `begin`, `end` (Iterators).

Non-member: `std::begin`/`std::end` overloads (C++11). Free function
templates overloaded for `initializer_list`: `rbegin`/`crbegin`,
`rend`/`crend` (C++14), `empty`, `data` (C++17).

### See also

- **span** (C++20) — a non-owning view over a contiguous sequence of
  objects
- **basic_string_view** (C++17) — read-only string view

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list*
