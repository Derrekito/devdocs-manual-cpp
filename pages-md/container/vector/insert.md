# std::vector<T,Allocator>::insert

Inserts one element, `count` copies, a `[first,last)` range, or an
initializer list before `pos`, shifting everything from `pos` onward.
Returns an iterator to the first inserted element (or `pos` if nothing
was inserted). If this makes `size()` exceed the old `capacity()`, every
iterator and reference into the vector is invalidated; otherwise only
the ones at or after `pos` are — anything before `pos` stays valid.

```cpp skip
it = v.insert(pos, value);              // copy
it = v.insert(pos, std::move(value));   // move                (C++11)
it = v.insert(pos, count, value);       // count copies of value
it = v.insert(pos, first, last);        // copy a [first,last) range
it = v.insert(pos, {a, b, c});          // from an initializer_list (C++11)
```

### What you provide

- **pos** — a `const_iterator` before which to insert; `end()` is valid.
- **value** — the element to insert (one, or `count` copies).
- **first, last** — a range to copy in; must not be iterators into this
  same vector (undefined behavior if they are).
- **ilist** — a `std::initializer_list<T>` to copy in.

### Guarantees and costs

- Single element: O(1) plus O(distance from `pos` to `end()`) for the
  shift.
- `count`/range/`ilist` forms: O(number inserted) plus that same shift
  cost.
- Reallocation (new `size()` > old `capacity()`): every iterator,
  pointer, and reference is invalidated, `end()` included. Otherwise,
  only iterators and references at or after `pos` are invalidated — the
  ones before it remain valid.
- Strong exception guarantee, with two carve-outs: an exception from an
  `InputIt` operation may leave the vector in a valid but unspecified
  state, and *(since C++11)* inserting a single element at the end may
  leave effects unspecified if `T`'s move constructor throws and `T`
  isn't CopyInsertable.

### Gotchas

- The returned iterator, and `pos` itself, are only good until the next
  mutation — the classic bug is reusing `pos` after an `insert` call
  that reallocated (see the example below).
- Inserting one element at a time in a loop costs O(N) per call; prefer
  the range or `count` overload, or `reserve()` first, to avoid repeated
  shifting or reallocation.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

void print(int id, const std::vector<int>& c)
{
    std::cout << id << ". ";
    for (int x : c)
        std::cout << x << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> c1(3, 100);
    print(1, c1);

    auto it = c1.begin();
    it = c1.insert(it, 200);
    print(2, c1);

    c1.insert(it, 2, 300);
    print(3, c1);

    // `it` no longer valid, get a new one:
    it = c1.begin();

    std::vector<int> c2(2, 400);
    c1.insert(std::next(it, 2), c2.begin(), c2.end());
    print(4, c1);

    c1.insert(c1.end(), {501, 502, 503});
    print(5, c1);
}
```

```text
1. 100 100 100 
2. 200 100 100 100 
3. 300 300 200 100 100 100 
4. 300 300 400 400 200 100 100 100 
5. 300 300 400 400 200 100 100 100 501 502 503 
```

### Reference

```cpp skip
iterator insert( const_iterator pos, const T& value );  // (until C++20)
constexpr iterator insert( const_iterator pos, const T& value );  // (since C++20)
iterator insert( const_iterator pos, T&& value );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos, T&& value );  // (since C++20)
iterator insert( const_iterator pos, size_type count, const T& value );  // (until C++20)
constexpr iterator
    insert( const_iterator pos, size_type count, const T& value );  // (since C++20)
template< class InputIt >
iterator insert( const_iterator pos, InputIt first, InputIt last );  // (until C++20)
template< class InputIt >
constexpr iterator insert( const_iterator pos, InputIt first, InputIt last );  // (since C++20)
iterator insert( const_iterator pos, std::initializer_list<T> ilist );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos,
                           std::initializer_list<T> ilist );  // (since C++20)
```

The range overload participates in overload resolution only when
`InputIt` qualifies as LegacyInputIterator *(since C++11;* before that it
degrades to the `count` overload for an integral `InputIt)`. Type
requirements: the copy and move forms need CopyInsertable+CopyAssignable
or MoveInsertable+MoveAssignable respectively; the range and
initializer-list forms need EmplaceConstructible, plus *(since C++17)*
Swappable, MoveAssignable, MoveConstructible, and MoveInsertable. Defect
reports: LWG 149 made the `count`/range overloads return an iterator
instead of nothing; LWG 247 extended the complexity guarantee to all
overloads; LWG 406 narrowed the strong exception guarantee to exclude
failures from an `InputIt` operation.

### See also

- **emplace** (C++11) — constructs an element in place at a position
- **push_back** — appends an element, or its move, to the end
- **erase** — removes elements
- **inserter** — creates a `std::insert_iterator` for use with algorithms

---
*Source: https://en.cppreference.com/w/cpp/container/vector/insert*
