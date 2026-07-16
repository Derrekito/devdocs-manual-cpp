# std::multimap

```cpp
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class multimap;  // (1)
namespace pmr {
    template<
        class Key,
        class T,
        class Compare = std::less<Key>
    > using multimap = std::multimap<Key, T, Compare,
        std::pmr::polymorphic_allocator<std::pair<const Key, T>>>;
}  // (2) (since C++17)
```

`std::multimap` is an associative container that contains a sorted list of
key-value pairs, while permitting multiple entries with the same key. Sorting is
done according to the comparison function `Compare`, applied to the keys.
Search, insertion, and removal operations have logarithmic complexity.

Iterators of `std::multimap` iterate in non-descending order of keys, where
non-descending is defined by the comparison that was used for construction. That
is, given

- `m`, a `std::multimap`
- `it_l` and `it_r`, dereferenceable iterators to `m`, with `it_l < it_r`.

`m.value_comp()(*it_r, *it_l) == false` (least to greatest if using the default
comparison).

The order of the key-value pairs whose keys compare equivalent is the order of
insertion and does not change.
*(since C++11)*

Everywhere the standard library uses the Compare requirements, equivalence is
determined by using the equivalence relation as described on Compare. In
imprecise terms, two objects `a` and `b` are considered equivalent if neither
compares less than the other: `!comp(a, b) && !comp(b, a)`.

`std::multimap` meets the requirements of Container, AllocatorAwareContainer,
AssociativeContainer and ReversibleContainer.

### Template parameters

### Member types

- **`key_type`** — `Key`
- **`mapped_type`** — `T`
- **`value_type`** — std::pair<const Key, T>
- **`size_type`** — Unsigned integer type (usually `std::size_t`)
- **`difference_type`** — Signed integer type (usually `std::ptrdiff_t`)
- **`key_compare`** — `Compare`
- **`allocator_type`** — `Allocator`
- **`reference`** — `value_type&`
- **`const_reference`** — const value_type&
- **`pointer`** — `Allocator::pointer` (until C++11)
  std::allocator_traits<Allocator>::pointer (since C++11)
- **`Allocator::pointer`** — (until C++11)
- **std::allocator_traits<Allocator>::pointer** — (since C++11)
- **`const_pointer`** — `Allocator::const_pointer` (until C++11)
  std::allocator_traits<Allocator>::const_pointer (since C++11)
- **`Allocator::const_pointer`** — (until C++11)
- **std::allocator_traits<Allocator>::const_pointer** — (since C++11)
- **`iterator`** — LegacyBidirectionalIterator to `value_type`
- **`const_iterator`** — LegacyBidirectionalIterator to const value_type
- **`reverse_iterator`** — std::reverse_iterator<iterator>
- **`const_reverse_iterator`** — std::reverse_iterator<const_iterator>
- **`node_type` (since C++17)** — a specialization of node handle representing a
  container node

### Member classes

- **value_compare** — compares objects of type `value_type` (class)

### Member functions

- **(constructor)** — constructs the `multimap` (public member function)
- **(destructor)** — destructs the `multimap` (public member function)
- **operator=** — assigns values to the container (public member function)
- **get_allocator** — returns the associated allocator (public member function)

**Iterators**

- **begincbegin (C++11)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)** — returns an iterator to the end (public member function)
- **rbegincrbegin (C++11)** — returns a reverse iterator to the beginning
  (public member function)
- **rendcrend (C++11)** — returns a reverse iterator to the end (public member
  function)

**Capacity**

- **empty** — checks whether the container is empty (public member function)
- **size** — returns the number of elements (public member function)
- **max_size** — returns the maximum possible number of elements (public member
  function)

**Modifiers**

- **clear** — clears the contents (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)
- **insert_range (C++23)** — inserts a range of elements (public member
  function)
- **emplace (C++11)** — constructs element in-place (public member function)
- **emplace_hint (C++11)** — constructs elements in-place using a hint (public
  member function)
- **erase** — erases elements (public member function)
- **swap** — swaps the contents (public member function)
- **extract (C++17)** — extracts nodes from the container (public member
  function)
- **merge (C++17)** — splices nodes from another container (public member
  function)

**Lookup**

- **count** — returns the number of elements matching specific key (public
  member function)
- **find** — finds element with specific key (public member function)
- **contains (C++20)** — checks if the container contains element with specific
  key (public member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given key (public member function)
- **upper_bound** — returns an iterator to the first element *greater* than the
  given key (public member function)

**Observers**

- **key_comp** — returns the function that compares keys (public member
  function)
- **value_comp** — returns the function that compares keys in objects of type
  `value_type` (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `multimaps` (function template)
- **std::swap(std::multimap)** — specializes the `std::swap` algorithm (function
  template)
- **erase_if(std::multimap) (C++20)** — erases all elements satisfying specific
  criteria (function template)

### Deduction guides
*(since C++17)*

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges construction and
      insertion for containers

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 230 | C++98 | `Key` was not required to be CopyConstructible (a key of
      type `Key` might not be able to be constructed) | `Key` is also required
      to be CopyConstructible

---
*Source: https://en.cppreference.com/w/cpp/container/multimap*
