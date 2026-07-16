# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::insert

```cpp
iterator insert( const value_type& value );  // (1) (since C++11)
iterator insert( value_type&& value );  // (2) (since C++11)
iterator insert( const_iterator hint, const value_type& value );  // (3) (since C++11)
iterator insert( const_iterator hint, value_type&& value );  // (4) (since C++11)
template< class InputIt >
void insert( InputIt first, InputIt last );  // (5) (since C++11)
void insert( std::initializer_list<value_type> ilist );  // (6) (since C++11)
iterator insert( node_type&& nh );  // (7) (since C++17)
iterator insert( const_iterator hint, node_type&& nh );  // (8) (since C++17)
```

Inserts element(s) into the container.

1,2) Inserts `value`.

3,4) Inserts `value`, using `hint` as a non-binding suggestion to where the
   search should start.

5) Inserts elements from range `[``first``,``last``)`.

6) Inserts elements from initializer list `ilist`.

7) If `nh` is an empty node handle, does nothing. Otherwise, inserts the element
   owned by `nh` into the container and returns an iterator pointing at the
   inserted element. The behavior is undefined if `nh` is not empty and
   `get_allocator() != nh.get_allocator()`.

8) If `nh` is an empty node handle, does nothing and returns the end iterator.
   Otherwise, inserts the element owned by `nh` into the container, and returns
   the iterator pointing to the element with key equivalent to `nh.key()`.
   `hint` is used as a non-binding suggestion to where the search should start.
   The behavior is undefined if `nh` is not empty and `get_allocator() !=
   nh.get_allocator()`.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated. If the insertion is
successful, pointers and references to the element obtained while it is held in
the node handle are invalidated, and pointers and references obtained to that
element before it was extracted become valid.(since C++17)

### Parameters

- **hint** — iterator, used as a suggestion as to where to insert the content
- **value** — element value to insert
- **first, last** — range of elements to insert
- **ilist** — initializer list to insert the values from
- **nh** — a compatible node handle

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Return value

1-4) Returns an iterator to the inserted element.

5,6) (none)

7,8) End iterator if `nh` was empty, iterator pointing to the inserted element
   otherwise.

### Exceptions

1-4) If an exception is thrown by any operation, the insertion has no effect.

### Complexity

1-4) Average case: `O(1)`, worst case `O(size())`.

5,6) Average case: `O(N)`, where N is the number of elements to insert. Worst
   case: `O(N * size() + N)`.

7,8) Average case: `O(1)`, worst case `O(size())`.

### Example

### See also

- **emplace** — constructs element in-place (public member function)
- **emplace_hint** — constructs elements in-place using a hint (public member
  function)
- **inserter** — creates a `std::insert_iterator` of type inferred from the
  argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/insert*
