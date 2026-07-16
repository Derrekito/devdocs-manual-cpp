# std::vector<T,Allocator>::resize

Changes `size()` to `count`: default-inserting or copying `value` to
fill new elements when growing, destroying the trailing elements when
shrinking. Unlike `reserve()`, this changes `size()` — and growing past
`capacity()` still reallocates exactly as `push_back` would; shrinking
never reduces capacity.

```cpp skip
v.resize(count);         // grow: default-insert; shrink: drop the tail
v.resize(count, value);  // grow: append copies of value
```

### What you provide

- **count** — the new `size()`.
- **value** — *(overload 2)* what to copy into any newly added elements;
  overload 1 default-inserts them instead.

### Guarantees and costs

- Linear in the difference between the old `size()` and `count`; growing
  past `capacity()` adds a reallocation on top.
- Growing (`count > size()`): appends `count - size()` new elements
  (default-inserted, or copies of `value`); may reallocate exactly like
  `insert`/`push_back` if that exceeds `capacity()`.
- Shrinking (`count < size()`): destroys the trailing elements; capacity
  is never reduced — invalidation-wise this is equivalent to that many
  `pop_back()` calls, not to a reallocation.
- `count == size()`: no-op.
- Strong exception guarantee. Although not explicitly specified,
  `std::length_error` is typically thrown if the required capacity would
  exceed `max_size()`. Overload 1 has the same *(since C++11)*
  move-constructor fallback caveat as `push_back`: if `T`'s move
  constructor isn't `noexcept` and `T` isn't CopyInsertable, a throw
  during the fallback leaves effects unspecified.

### Gotchas

- Overload 1's default-insertion value-initializes elements — zeroing
  scalars, default-constructing class types. If you need to skip that
  initialization for scalars, `resize()` can't; you'd need a custom
  allocator.
- Growing reallocates exactly as `push_back` would — the same
  "everything invalidated" rule applies, so don't hold iterators across
  a `resize()` call that might grow.
- Shrinking keeps the old capacity — `resize()` cannot be used to
  release memory; call `shrink_to_fit()` afterward if you want that.

### Example

```cpp
#include <iostream>
#include <vector>

void print(const char* label, const std::vector<int>& c)
{
    std::cout << label;
    bool first = true;
    for (int x : c)
    {
        if (!first)
            std::cout << ' ';
        std::cout << x;
        first = false;
    }
    std::cout << '\n';
}

int main()
{
    std::vector<int> c = {1, 2, 3};
    print("holds: ", c);

    c.resize(5);
    print("resize(5): ", c);

    c.resize(2);
    print("resize(2): ", c);

    c.resize(6, 4);
    print("resize(6, 4): ", c);
}
```

```text
holds: 1 2 3
resize(5): 1 2 3 0 0
resize(2): 1 2
resize(6, 4): 1 2 4 4 4 4
```

### Reference

```cpp skip
void resize( size_type count );  // (1) (constexpr since C++20)
void resize( size_type count, const value_type& value );  // (2) (constexpr since C++20)
```

Overload 1 requires `T` to be MoveInsertable and DefaultInsertable;
overload 2 requires `T` to be CopyInsertable. Defect reports: LWG 679
changed `value` to be passed by const reference instead of by value; LWG
1525 specified the previously-unspecified behavior of `resize(size())`;
LWG 2033 switched element removal to use `pop_back()` (previously
`erase()`) and corrected `T`'s type requirements; LWG 2066 gave overload
1 the same exception-safety guarantee as overload 2.

### See also

- **size** — the current number of elements
- **reserve** — grows capacity without changing size
- **insert** — adds elements at an arbitrary position
- **erase** — removes elements at an arbitrary position

---
*Source: https://en.cppreference.com/w/cpp/container/vector/resize*
