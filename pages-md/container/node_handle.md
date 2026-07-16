# Node handle (C++17)

```cpp
template</* unspecified */>
class /* node-handle */;  // (since C++17)
```

Associative containers `std::set`, `std::map`, `std::multiset`, `std::multimap`,
`std::unordered_set`, `std::unordered_map`, `std::unordered_multiset`,
`std::unordered_multimap` are node-based data structures, and their nodes can be
extracted as an object of unspecified type known as *node handle*.

Node handle is a move-only type that owns and provides access to the element
(the `value_type`) stored in the node, and provides non-const access to the key
part of the element (the `key_type`) and the mapped part of the element (the
`mapped_type`). If the node handle destructs while holding the node, the node is
properly destructed using the appropriate allocator for the container. The node
handle contains a copy of the container’s allocator. This is necessary so that
the node handle can outlive the container.

The exact type of node handle (shown here as /* node-handle */) is unspecified,
but each container exposes its node handle type as the member `node_type`.

Node handles can be used to transfer ownership of an element between two
associative containers with the same key, value, and allocator type (ignoring
comparison or hash/equality), without invoking any copy/move operations on the
container element (this kind of operation is known as "splicing"). Transfer
between unique and non-unique containers is also permitted: a node handle from a
`std::map` can be inserted into an `std::multimap`, but not into
`std::unordered_map` or `std::set`.

A node handle may be empty, in which case it holds no element and no allocator.
The default-constructed and moved-from node handle is empty. In addition, an
empty node handle can be produced by a failed call to container member function
`extract`.

Pointers and references to an element that are obtained while it is owned by a
node handle are invalidated if the element is successfully inserted into a
container.

For all map containers (`std::map`, `std::multimap`, `std::unordered_map`, and
`std::unordered_multimap`) whose `key_type` is `K` and `mapped_type` is `T`, the
behavior of operations involving node handles is undefined if a user-defined
specialization of `std::pair` exists for std::pair<K, T> or std::pair<const K,
T>.

### Member types

- **`key_type`(map containers only)** — the key stored in the node
- **`mapped_type`(map containers only)** — the mapped part of the element stored
  in the node
- **`value_type`(set containers only)** — the element stored in the node
- **`allocator_type`** — the allocator to be used when destroying the element

### Member functions

## constructors

```cpp
constexpr /* node-handle */() noexcept;  // (1)
/* node-handle */ (/* node-handle */&& nh) noexcept;  // (2)
```

1) The default constructor initializes the node handle to the empty state.

2) The move constructor takes ownership of the container element from `nh`,
   move-constructs the member allocator, and leaves `nh` in the empty state.

### Parameters

- **nh** — a node handle with the same type (not necessarily the same container)

### Notes

Node handles are move-only, the copy constructor is not defined.

## operator=

```cpp
/* node-handle */& operator=(/* node-handle */&& nh);
```

- If the node handle is not empty,
- Acquires ownership of the container element from `nh`;
- If node handle was empty (and so did not contain an allocator) or if
  std::allocator_traits<allocator_type>::propagate_on_container_move_assignment
  is `true`, move-assigns the allocator from `nh`;
- sets `nh` to the empty state.

The behavior is undefined if the node is not empty and
std::allocator_traits<allocator_type>::propagate_on_container_move_assignment is
`false` and the allocators do not compare equal.

### Parameters

- **nh** — node handle with the same type (not necessarily the same container)

### Return

`*this`

### Exceptions

Throws nothing.

### Notes

Node handles are move-only, the copy assignment is not defined.

## destructor

```cpp
~/* node-handle */();
```

- If the node handle is not empty,

## empty

```cpp
bool empty() const noexcept;  // (until C++20)
[[nodiscard]] bool empty() const noexcept;  // (since C++20)
```

Returns `true` if the node handle is empty, `false` otherwise.

## operator bool

```cpp
explicit operator bool() const noexcept;
```

Converts to `false` if the node handle is empty, `true` otherwise.

## get_allocator

```cpp
allocator_type get_allocator() const;
```

Returns a copy of the stored allocator (which is a copy of the allocator of the
source container). The behavior is undefined if the node handle is empty.

### Exceptions

Throws nothing.

## value

```cpp
value_type& value() const;  // (set containers only)
```

Returns a reference to the `value_type` subobject in the container element
object managed by this node handle. The behavior is undefined if the node handle
is empty.

### Exceptions

Throws nothing.

## key

```cpp
key_type& key() const;  // (map containers only)
```

Returns a non-const reference to the `key_type` member of the `value_type`
subobject in the container element object managed by this node handle. The
behavior is undefined if the node handle is empty.

### Exceptions

Throws nothing.

### Notes

This function makes it possible to modify the key of a node extracted from a
map, and then re-insert it into the map, without ever copying or moving the
element.

## mapped

```cpp
mapped_type& mapped() const;  // (map containers only)
```

Returns a reference to the `mapped_type` member of the `value_type` subobject in
the container element object managed by this node handle. The behavior is
undefined if the node handle is empty.

### Exceptions

Throws nothing.

## swap

```cpp
void swap(/* node-handle */& nh) noexcept(/* see below */);
```

- swaps ownership of container nodes;
- if one node is empty or if both nodes are non-empty and
  std::allocator_traits<allocator_type>::propagate_on_container_swap is `true`,
  swaps the allocators as well.

The behavior is undefined if both nodes are not empty and
std::allocator_traits<allocator_type>::propagate_on_container_swap is `false`
and the allocators do not compare equal.

### Exceptions

`noexcept` specification:

`noexcept(std::allocator_traits<allocator_type>::propagate_on_container_swap::value
|| std::allocator_traits<allocator_type>::is_always_equal::value)`

### Non-member functions

## swap

```cpp
friend void swap(/* node-handle */& x, /* node-handle */& y) noexcept(noexcept(x.swap(y)));
```

Effectively executes `x.swap(y)`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `node-handle` is an
associated class of the arguments.

---
*Source: https://en.cppreference.com/w/cpp/container/node_handle*
