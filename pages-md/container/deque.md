# std::deque

`std::deque` (double-ended queue) is a sequence container with cheap
O(1) push and pop at **both** ends, plus O(1) random access — the
usual choice when you need a growable buffer that's pushed or popped
from the front as often as the back (a `vector` only does that cheaply
at the back). Unlike `vector`, its elements are not stored
contiguously: a typical implementation is a sequence of fixed-size
chunks, so indexed access costs two pointer dereferences instead of
one, and there's no `data()`.

```cpp skip
std::deque<int> d;
d.push_back(x); d.push_front(x);            // O(1) at either end
d.emplace_back(a...); d.emplace_front(a...); // in-place        (since C++11)
d.pop_back(); d.pop_front();                 // O(1) at either end
d[i]; d.at(i);                               // O(1) random access
d.front(); d.back();
d.insert(pos, x); d.erase(pos);              // O(n) away from the ends
```

### Template parameters

- **T** — element type. Until C++11, must be CopyAssignable and
  CopyConstructible; since C++11, must be a complete type meeting
  Erasable (individual operations require more).
- **Allocator** — defaults to `std::allocator<T>`; must meet the
  Allocator requirements. A mismatched `Allocator::value_type` is UB
  until C++20 and ill-formed from C++20 on.

### Guarantees and costs

- Random access (`operator[]`, `at`): O(1), but costs two pointer
  dereferences versus one for `vector`, because storage is chunked
  rather than contiguous.
- `push_front`/`push_back`/`pop_front`/`pop_back`: O(1). Insertion or
  removal anywhere else: O(n).
- Growing a deque never copies existing elements (unlike `vector`'s
  reallocation), but the minimum footprint is large: even a
  one-element deque allocates a full internal chunk (around 8x the
  object size on 64-bit libstdc++; 16x, or 4096 bytes, whichever is
  larger, on libc++).
- Iterators: any read-only operation leaves them alone; `clear`,
  `insert`, `emplace`, `push_front`, `push_back`, `emplace_front`,
  `emplace_back`, and `shrink_to_fit` invalidate all of them; `swap`
  may invalidate the past-the-end iterator (implementation-defined).
  `erase` invalidates iterators to the erased elements, plus the
  past-the-end iterator unless the erased range is at the front and
  the back is untouched (since C++11). `resize` to a smaller size
  invalidates only iterators to the removed elements and the
  past-the-end iterator; to a larger size, it invalidates everything.
- References and pointers are sturdier than iterators: `insert`,
  `emplace`, `push_front`/`push_back`, `erase`, and `pop_front`/
  `pop_back` at either end never invalidate references to elements
  that aren't erased — only the iterators move.
- Meets Container, AllocatorAwareContainer, SequenceContainer, and
  ReversibleContainer.

### Gotchas

- No `data()` — storage isn't contiguous, so a deque can't be handed
  to an API expecting a flat `T*` buffer; use `vector` for that.
- `push_front`/`push_back` invalidate **all iterators**, even though
  references and pointers to existing elements survive — an iterator
  taken before the push is not safe to reuse, even if the object it
  pointed to is still alive.
- Want a restricted FIFO or LIFO interface instead of full deque
  access? `std::queue`/`std::stack` wrap a deque by default and hide
  the operations you shouldn't be using directly.

### Example

```cpp
#include <deque>
#include <iostream>

int main()
{
    std::deque<int> d{7, 5, 16, 8};

    d.push_front(13);
    d.push_back(25);

    for (int n : d)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
13 7 5 16 8 25
```

### Reference

```cpp skip
template<
    class T,
    class Allocator = std::allocator<T>
> class deque;
namespace pmr {
    template< class T >
    using deque = std::deque<T, std::pmr::polymorphic_allocator<T>>;
}  // (since C++17)
```

Member functions, condensed:

- **(constructor)**, **(destructor)**, **operator=** — lifetime and
  assignment.
- **assign** — replaces the contents; **assign_range** (C++23) does it
  from a range.
- **get_allocator** — returns the associated allocator.
- **at / operator[]** — bounds-checked / unchecked access.
- **front / back** — access the first / last element.
- **begin/cbegin, end/cend, rbegin/crbegin, rend/crend** — iterators
  (`c`-prefixed forms since C++11).
- **empty / size / max_size** — capacity queries.
- **shrink_to_fit** — releases unused capacity (added by a defect
  report).
- **clear** — removes all elements.
- **insert** / **insert_range** (C++23) / **emplace** (C++11) — add
  elements at a position.
- **erase** — removes elements at a position.
- **push_back** / **emplace_back** (C++11) / **append_range** (C++23)
  — add to the end.
- **pop_back** — removes the last element.
- **push_front** / **emplace_front** (C++11) / **prepend_range**
  (C++23) — add to the beginning.
- **pop_front** — removes the first element.
- **resize** — changes the number of elements.
- **swap** — swaps contents with another deque.

Non-member: lexicographic comparison operators (`==`/`!=`/`<`/`<=`/
`>`/`>=` removed in C++20 in favor of `<=>`); `std::swap`;
`std::erase`/`std::erase_if` (C++20).

Feature-test macro `__cpp_lib_containers_ranges` (`202202L`, C++23)
marks ranges-based construction and insertion.

Defect report: LWG 230 retroactively required `T` to be
CopyConstructible for C++98 (previously an element type that couldn't
be constructed was technically allowed).

### See also

- **queue** — FIFO adaptor; uses `deque` as its default underlying
  container

---
*Source: https://en.cppreference.com/w/cpp/container/deque*
