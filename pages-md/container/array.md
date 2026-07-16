# std::array

`std::array` is a fixed-size array wrapped in a container: the same
layout and performance as a C-style `T[N]`, but with a container's
interface (`size()`, iterators, bounds-checked `at()`) and real value
semantics — it copies, assigns, and returns by value instead of
decaying to a pointer. Reach for it whenever the size is known at
compile time; once you need to grow or shrink at runtime, use
`std::vector` instead.

```cpp skip
std::array<int, 3> a{1, 2, 3};   // aggregate init          (since C++11)
std::array a{1, 2, 3};           // CTAD: deduces <int, 3>  (since C++17)
a[i];                             // unchecked access
a.at(i);                          // bounds-checked, throws std::out_of_range
a.front(); a.back();              // first / last element
a.size(); a.empty();              // both are compile-time constants
a.fill(0);                        // set every element
a.data();                         // pointer to contiguous storage
```

### Template parameters

- **T** — element type; must be MoveConstructible and MoveAssignable.
- **N** — the number of elements, or `0`.

### Guarantees and costs

- Storage is contiguous (`std::array` satisfies ContiguousContainer
  since C++17), so `data()` gives a real `T*` usable with C APIs — a C
  array has this too, but `std::array` also owns its size.
- Element access, `size()`, `front()`/`back()`: O(1).
- `swap()` is O(N), **linear** — unlike most containers, where swap is
  O(1) — because the elements themselves are exchanged, not a pointer
  to a buffer.
- Iterators are never invalidated for the array's lifetime. The
  exception: after `swap()`, an iterator still refers to the same
  *position*, so its value changes.

### Gotchas

- `std::array<int, N> a;` (no braces) default-initializes: for a
  non-class `T` this leaves every element **indeterminate**. Write
  `std::array<int, N> a{};` to zero-initialize instead.
- Calling `front()` or `back()` on a zero-length array (`N == 0`) is
  undefined behavior.
- The size is baked into the type — `std::array<int, 3>` and
  `std::array<int, 4>` don't convert to each other and can't be
  resized.

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>

int main()
{
    std::array<int, 5> a{5, 3, 1, 4, 2};

    std::sort(a.begin(), a.end());

    std::cout << "size=" << a.size() << " front=" << a.front()
              << " back=" << a.back() << '\n';
    for (int x : a)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
size=5 front=1 back=5
1 2 3 4 5
```

### Reference

```cpp skip
template<
    class T,
    std::size_t N
> struct array;  // (since C++11)
```

`std::array` satisfies the requirements of Container and
ReversibleContainer, except that a default-constructed array is not
empty and `swap` is linear; it satisfies ContiguousContainer (since
C++17); and it partially satisfies SequenceContainer. It can also be
used as a tuple of `N` elements of the same type.

Member functions, condensed:

- **(constructor)** (implicit) — aggregate-initializes the array.
- **(destructor)** (implicit) — destroys every element.
- **operator=** (implicit) — overwrites every element from another
  array.
- **at** — bounds-checked access, throws `std::out_of_range`.
- **operator[]** — unchecked access.
- **front / back** — access the first / last element.
- **data** — pointer to the contiguous storage.
- **begin/cbegin, end/cend, rbegin/crbegin, rend/crend** — iterators
  (the `c`-prefixed forms since C++11).
- **empty** — true only when `N == 0`.
- **size / max_size** — both equal `N`, evaluated at compile time.
- **fill** — assigns one value to every element.
- **swap** — exchanges contents element-wise, O(N).

Non-member: lexicographic `operator==`/`!=`/`<`/`<=`/`>`/`>=` (C++11;
the ordering operators are removed in C++20 in favor of `<=>`, added
C++20); `get<I>(array)` (C++11) accesses one element; `std::swap`
(C++11); `to_array` (C++20) builds a `std::array` from a C array,
deducing type and size.

### See also

- **make_array** (library fundamentals TS v2) — creates a `std::array`
  whose size and element type are deduced from the arguments

---
*Source: https://en.cppreference.com/w/cpp/container/array*
