# std::extents, std::dextents

```cpp
template< class IndexType, std::size_t... Extents >
class extents;  // (1) (since C++23)
template< class IndexType, std::size_t Rank >
using dextents = /* see below */  // (2) (since C++23)
```

1) Represents a multidimensional index space of rank equal to
   `sizeof...(Extents)`.

2) A convenient alias template for an all-dynamic `extents`. Let `d` denote
   `std::dynamic_extent`, each specialization of it `dextents<TypeIndex, Rank>`
   is equivalent to `extents<IndexType /*, d, d, ..., d*/>` (i.e. `d` is
   repeated a total of `Rank` times).

Each specialization of `extents` models `regular` and is TriviallyCopyable.

### Template parameters

- **IndexType** — the type of each non-dynamic `Extents`. Shall be a signed or
  unsigned integer type. Otherwise, the program is ill-formed
- **Extents** — represents extent (size of an integer interval) for each rank
  index. Each element of it is either equal to std::dynamic_extent (in this
  case, it represents a *dynamic extent* and the extent size will be determined
  dynamically), or is representable as a value of type `IndexType` (then it
  represents a *static extent* and the extent size is just the value of it), or
  else the program is ill-formed
- **Rank** — denotes the rank of an all-dynamic `extents`

### Member types

- **`index_type`** — `IndexType`
- **`size_type`** — std::make_unsigned_t<index_type>
- **`rank_type`** — `std::size_t`

### Data members

- **`dynamic-extents` (private)** — An array of type `std::array<index_type,
  rank_dynamic()>`, which stores the size of each dynamic extents.
  (exposition-only member object*)

### Member functions

- **(constructor)** — constructs an `extents` (public member function)

**Observers**

- **rank [static]** — returns the static rank of an `extents` (public static
  member function)
- **rank_dynamic [static]** — returns the dynamic rank of an `extents` (public
  static member function)
- **static_extent [static]** — returns static extent size of an `extents` at a
  certain rank index (public static member function)
- **extent** — returns dynamic extent size of an `extents` at a certain rank
  index (public member function)

**Helpers**

- ***fwd-prod-of-extents*** — returns the product of extent size in range
  `[`​0​`,`i`)` (exposition-only member function*)
- ***rev-prod-of-extents*** — returns the product of extents size in range `[`i
  + 1`,`rank()`)` (exposition-only member function*)
- ***index-cast*** — casts the input into an integer type (exposition-only
  member function*)
- ***dynamic-index* (private)** — returns the number of dynamic extents below a
  certain rank index (exposition-only member function*)
- ***dynamic-index-inv* (private)** — returns the number `r` such that in range
  `[`​0​`,`r + 1`)` there is exactly `i + 1` dynamic extents for a certain
  argument `i` (exposition-only member function*)

### Non-member functions

- **operator== (C++23)** — compare underlying extents in each dimension of two
  `extents` (function)

### Deduction guides(C++23)

### Example

### See also

- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents*
