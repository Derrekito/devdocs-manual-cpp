# std::flat_multiset

```cpp
template<
    class Key,
    class Compare = std::less<Key>,
    class KeyContainer = std::vector<Key>
> class flat_multiset;
```

The flat multiset is a container adaptor that gives the functionality of an
associative container that stores a sorted set of objects of type `Key`. Unlike
`std::flat_set`, multiple keys with equivalent values are allowed. Sorting is
done using the key comparison function `Compare`.

The class template `flat_multiset` acts as a wrapper to the underlying sorted
container passed as object of type `KeyContainer`.

Everywhere the standard library uses the Compare requirements, uniqueness is
determined by using the equivalence relation. Informally, two objects `a` and
`b` are considered equivalent if neither compares less than the other: `!comp(a,
b) && !comp(b, a)`.

`std::flat_multiset` meets the requirements of Container, ReversibleContainer,
optional container requirements, and all requirements of AssociativeContainer
(including logarithmic search complexity), except that:

- requirements related to nodes are not applicable,
- iterator invalidation requirements differ,
- the complexity of insertion and erasure operations is linear.

A flat multiset supports most AssociativeContainer's operations that use equal
keys.

### Iterator invalidation

### Template parameters

- **Key** — The type of the stored elements. The program is ill-formed if `Key`
  is not the same type as `KeyContainer::value_type`.
- **Compare** — A Compare type providing a strict weak ordering.
- **KeyContainer** — The type of the underlying SequenceContainer to store the
  elements. The iterators of such container should satisfy
  LegacyRandomAccessIterator or model `random_access_iterator`. The standard
  containers `std::vector` and `std::deque` satisfy these requirements.

### Member types

- **`container_type`** — `Key``Container`
- **`key_type`** — `Key`
- **`value_type`** — `Key`
- **`key_compare`** — `Compare`
- **`value_compare`** — `Compare`
- **`reference`** — `value_type&`
- **`const_reference`** — const value_type&
- **`size_type`** — typename KeyContainer::size_type
- **`difference_type`** — typename KeyContainer::difference_type
- **`iterator`** — implementation-defined LegacyRandomAccessIterator and
  `random_access_iterator` to `value_type`
- **`const_iterator`** — implementation-defined LegacyRandomAccessIterator and
  `random_access_iterator` to const value_type
- **`reverse_iterator`** — std::reverse_iterator<iterator>
- **`const_reverse_iterator`** — std::reverse_iterator<const_iterator>

### Member objects

- **`c` (private)** — the underlying container of `container_type`
  (exposition-only member object*)
- **`compare` (private)** — the comparison function object of type `key_compare`
  (exposition-only member object*)

### Member functions

- **(constructor)** — constructs the `flat_multiset` (public member function)
- **(destructor)** — destructs the `flat_multiset` (public member function)
- **operator=** — assigns values to the container adaptor (public member
  function)

**Iterators**

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend** — returns an iterator to the end (public member function)
- **rbegincrbegin** — returns a reverse iterator to the beginning (public member
  function)
- **rendcrend** — returns a reverse iterator to the end (public member function)

**Capacity**

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **size** — returns the number of elements (public member function)
- **max_size** — returns the maximum possible number of elements (public member
  function)

**Modifiers**

- **emplace** — constructs element in-place (public member function)
- **emplace_hint** — constructs elements in-place using a hint (public member
  function)
- **insert** — inserts elements (public member function)
- **insert_range** — inserts a range of elements (public member function)
- **extract** — extracts the underlying container (public member function)
- **replace** — replaces the underlying container (public member function)
- **erase** — erases elements (public member function)
- **swap** — swaps the contents (public member function)
- **clear** — clears the contents (public member function)

**Lookup**

- **find** — finds element with specific key (public member function)
- **count** — returns the number of elements matching specific key (public
  member function)
- **contains** — checks if the container contains element with specific key
  (public member function)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given key (public member function)
- **upper_bound** — returns an iterator to the first element *greater* than the
  given key (public member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

**Observers**

- **key_comp** — returns the function that compares keys (public member
  function)
- **value_comp** — returns the function that compares keys in objects of type
  `value_type` (public member function)

### Non-member functions

- **operator==operator<=> (C++23)** — lexicographically compares the values of
  two `flat_multisets` (function template)
- **std::swap(std::flat_multiset) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **erase_if(std::flat_multiset) (C++23)** — erases all elements satisfying
  specific criteria (function template)

### Helper classes

- **std::uses_allocator<std::flat_multiset> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

### Tags

- **sorted_equivalentsorted_equivalent_t (C++23)** — a tag used to indicate that
  elements of a container or range are sorted (uniqueness is not required) (tag)

### Deduction guides

### Notes

The member types `iterator` and `const_iterator` may be aliases to the same
type. This means defining a pair of function overloads using the two types as
parameter types may violate the One Definition Rule. Since `iterator` is
convertible to `const_iterator`, a single function with a `const_iterator` as
parameter type will work instead.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_flat_set` | 202207L | (C++23) | `std::flat_set` and
      `std::flat_multiset`

### Example

### See also

- **flat_set (C++23)** — adapts a container to provide a collection of unique
  keys, sorted by keys (class template)
- **multiset** — collection of keys, sorted by keys (class template)
- **unordered_multiset (C++11)** — collection of keys, hashed by keys (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/flat_multiset*
