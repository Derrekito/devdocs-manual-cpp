# std::atomic_ref

```cpp
template< class T >
struct atomic_ref;  // (1) (since C++20)
template< class T >
struct atomic_ref<T*>;  // (2) (since C++20)
```

The `std::atomic_ref` class template applies atomic operations to the object it
references. For the lifetime of the `std::atomic_ref` object, the object it
references is considered an atomic object. If one thread writes to an atomic
object while another thread reads from it, the behavior is well-defined (see
memory model for details on data races). In addition, accesses to atomic objects
may establish inter-thread synchronization and order non-atomic memory accesses
as specified by `std::memory_order`.

The lifetime of an object must exceed the lifetime of all `std::atomic_ref`s
that references the object. While any `std::atomic_ref` instance referencing an
object exists, the object must be exclusively accessed through these
`std::atomic_ref` instances. No subobject of an object referenced by an
`std::atomic_ref` object may be concurrently referenced by any other
`std::atomic_ref` object.

Atomic operations applied to an object through an `std::atomic_ref` are atomic
with respect to atomic operations applied through any other `std::atomic_ref`
referencing the same object.

`std::atomic_ref` is CopyConstructible.

Like language references, constness is shallow for `std::atomic_ref` - it is
possible to modify the referenced value through a const `std::atomic_ref`
object.

### Specializations

#### Primary template

The primary `std::atomic_ref` template may be instantiated with any
TriviallyCopyable type `T` (including bool):

```cpp
struct Counters { int a; int b; }; // user-defined trivially-copyable type
alignas(std::atomic_ref<Counters>::required_alignment) Counters counter;
std::atomic_ref<Counters> cnt(counter); // specialization for the user-defined type
```

#### Partial specialization for pointer types

The standard library provides partial specializations of the `std::atomic_ref`
template for all pointer types. In addition to the operations provided for all
atomic types, these specializations additionally support atomic arithmetic
operations appropriate to pointer types, such as `fetch_add`, `fetch_sub`.

#### Specializations for integral types

When instantiated with one of the following integral types, `std::atomic_ref`
provides additional atomic operations appropriate to integral types such as
`fetch_add`, `fetch_sub`, `fetch_and`, `fetch_or`, `fetch_xor`:

- The character types char, char8_t, char16_t, char32_t, and wchar_t;
- The standard signed integer types: signed char, short, int, long, and long
  long;
- The standard unsigned integer types: unsigned char, unsigned short, unsigned
  int, unsigned long, and unsigned long long;
- Any additional integral types needed by the typedefs in the header
  `<cstdint>`.

Signed integer arithmetic is defined to use two's complement; there are no
undefined results.

#### Specializations for floating-point types

When instantiated with one of the cv-unqualified floating-point types (float,
double, long double and cv-unqualified extended floating-point types(since
C++23)), `std::atomic_ref` provides additional atomic operations appropriate to
floating-point types such as `fetch_add` and `fetch_sub`.

No operations result in undefined behavior even if the result is not
representable in the floating-point type. The floating-point environment in
effect may be different from the calling thread's floating-point environment.

### Member types

- **`value_type`** — *see below*
- **`difference_type`** — `value_type` (only for `atomic_ref<Integral>` and
  `atomic_ref<Floating>` specializations) `std::ptrdiff_t` (only for
  `std::atomic_ref<T*>` specializations)

For every `std::atomic_ref<X>` (whether or not specialized),
`std::atomic_ref<X>::value_type` is `X`.

`difference_type` is not defined in the primary `atomic_ref` template.

### Member functions

- **(constructor)** — constructs an `atomic_ref` object (public member function)
- **operator=** — stores a value into the object referenced by an `atomic_ref`
  object (public member function)
- **is_lock_free** — checks if the `atomic_ref` object is lock-free (public
  member function)
- **store** — atomically replaces the value of the referenced object with a
  non-atomic argument (public member function)
- **load** — atomically obtains the value of the referenced object (public
  member function)
- **operator T** — loads a value from the referenced object (public member
  function)
- **exchange** — atomically replaces the value of the referenced object and
  obtains the value held previously (public member function)
- **compare_exchange_weakcompare_exchange_strong** — atomically compares the
  value of the referenced object with non-atomic argument and performs atomic
  exchange if equal or atomic load if not (public member function)
- **wait (C++20)** — blocks the thread until notified and the atomic value
  changes (public member function)
- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function)

**Constants**

- **is_always_lock_free [static]** — indicates that the type is always lock-free
  (public static member constant)
- **required_alignment [static]** — indicates the required alignment of an
  object to be referenced by `atomic_ref` (public static member constant)

### Specialized member functions

- **fetch_add** — atomically adds the argument to the value stored in the
  referenced object and obtains the value held previously (public member
  function)
- **fetch_sub** — atomically subtracts the argument from the value stored in the
  referenced object and obtains the value held previously (public member
  function)
- **fetch_and** — atomically performs bitwise AND between the argument and the
  value of the referenced object and obtains the value held previously (public
  member function)
- **fetch_or** — atomically performs bitwise OR between the argument and the
  value of the referenced object and obtains the value held previously (public
  member function)
- **fetch_xor** — atomically performs bitwise XOR between the argument and the
  value of the referenced object and obtains the value held previously (public
  member function)
- **operator++operator++(int)operator--operator--(int)** — atomically increments
  or decrements the referenced object by one (public member function)
- **operator+=operator-=operator&=operator|=operator^=** — atomically adds,
  subtracts, or performs bitwise AND, OR, XOR with the referenced value (public
  member function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_atomic_ref` | 201806L | (C++20) | `std::atomic_ref`

### See also

- **atomic (C++11)** — atomic class template and specializations for bool,
  integral, floating-point,(since C++20) and pointer types (class template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref*
