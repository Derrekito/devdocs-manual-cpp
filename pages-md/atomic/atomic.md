# std::atomic

```cpp
template< class T >
struct atomic;  // (1) (since C++11)
template< class U >
struct atomic<U*>;  // (2) (since C++11)
template< class U >
struct atomic<std::shared_ptr<U>>;  // (3) (since C++20)
template< class U >
struct atomic<std::weak_ptr<U>>;  // (4) (since C++20)
#define _Atomic(T) /* see below */  // (5) (since C++23)
```

Each instantiation and full specialization of the `std::atomic` template defines
an atomic type. If one thread writes to an atomic object while another thread
reads from it, the behavior is well-defined (see memory model for details on
data races).

In addition, accesses to atomic objects may establish inter-thread
synchronization and order non-atomic memory accesses as specified by
`std::memory_order`.

`std::atomic` is neither copyable nor movable.

The compatibility macro `_Atomic` is provided in `<stdatomic.h>` such that
`_Atomic(T)` is identical to `std::atomic<T>` while both are well-formed.
It is unspecified whether any declaration in namespace `std` is available when
`<stdatomic.h>` is included.
*(since C++23)*

### Specializations

#### Primary template

The primary `std::atomic` template may be instantiated with any
TriviallyCopyable type `T` satisfying both CopyConstructible and CopyAssignable.
The program is ill-formed if any of following values is `false`:

- `std::is_trivially_copyable<T>::value`
- `std::is_copy_constructible<T>::value`
- `std::is_move_constructible<T>::value`
- `std::is_copy_assignable<T>::value`
- `std::is_move_assignable<T>::value`

```cpp
struct Counters { int a; int b; }; // user-defined trivially-copyable type
std::atomic<Counters> cnt;         // specialization for the user-defined type
```

std::atomic<bool> uses the primary template. It is guaranteed to be a standard
layout struct.

#### Partial specializations

The standard library provides partial specializations of the `std::atomic`
template for the following types with additional properties that the primary
template does not have:

2) Partial specializations `std::atomic<U*>` for all pointer types. These
   specializations have standard layout, trivial default constructors,(until
   C++20) and trivial destructors. Besides the operations provided for all
   atomic types, these specializations additionally support atomic arithmetic
   operations appropriate to pointer types, such as `fetch_add`, `fetch_sub`.
   3,4) Partial specializations std::atomic<std::shared_ptr<U>> and
   std::atomic<std::weak_ptr<U>> are provided for `std::shared_ptr` and
   `std::weak_ptr`. See `std::atomic<std::shared_ptr>` and
   `std::atomic<std::weak_ptr>` for details. (since C++20)

#### Specializations for integral types

When instantiated with one of the following integral types, `std::atomic`
provides additional atomic operations appropriate to integral types such as
`fetch_add`, `fetch_sub`, `fetch_and`, `fetch_or`, `fetch_xor`:

- The character types char, `char8_t`(since C++20), char16_t, char32_t, and
  wchar_t;
- The standard signed integer types: signed char, short, int, long, and long
  long;
- The standard unsigned integer types: unsigned char, unsigned short, unsigned
  int, unsigned long, and unsigned long long;
- Any additional integral types needed by the typedefs in the header
  `<cstdint>`.

Additionally, the resulting `std::atomic<Integral>` specialization has standard
layout, a trivial default constructor,(until C++20) and a trivial destructor.
Signed integer arithmetic is defined to use two's complement; there are no
undefined results.

#### Specializations for floating-point types
When instantiated with one of the cv-unqualified floating-point types (float,
double, long double and cv-unqualified extended floating-point types(since
C++23)), `std::atomic` provides additional atomic operations appropriate to
floating-point types such as `fetch_add` and `fetch_sub`.
Additionally, the resulting `std::atomic<Floating>` specialization has standard
layout and a trivial destructor.
No operations result in undefined behavior even if the result is not
representable in the floating-point type. The floating-point environment in
effect may be different from the calling thread's floating-point environment.
*(since C++20)*

### Type aliases

Type aliases are provided for bool and all integral types listed above, as
follows:

**Aliases for all `std::atomic<Integral>`**

- ****atomic_bool** (C++11)** — std::atomic<bool> (typedef)
- ****atomic_char** (C++11)** — std::atomic<char> (typedef)
- ****atomic_schar** (C++11)** — std::atomic<signed char> (typedef)
- ****atomic_uchar** (C++11)** — std::atomic<unsigned char> (typedef)
- ****atomic_short** (C++11)** — std::atomic<short> (typedef)
- ****atomic_ushort** (C++11)** — std::atomic<unsigned short> (typedef)
- ****atomic_int** (C++11)** — std::atomic<int> (typedef)
- ****atomic_uint** (C++11)** — std::atomic<unsigned int> (typedef)
- ****atomic_long** (C++11)** — std::atomic<long> (typedef)
- ****atomic_ulong** (C++11)** — std::atomic<unsigned long> (typedef)
- ****atomic_llong** (C++11)** — std::atomic<long long> (typedef)
- ****atomic_ullong** (C++11)** — std::atomic<unsigned long long> (typedef)
- ****atomic_char8_t** (C++20)** — std::atomic<char8_t> (typedef)
- ****atomic_char16_t** (C++11)** — std::atomic<char16_t> (typedef)
- ****atomic_char32_t** (C++11)** — std::atomic<char32_t> (typedef)
- ****atomic_wchar_t** (C++11)** — std::atomic<wchar_t> (typedef)
- ****atomic_int8_t** (C++11)(optional)** — std::atomic<std::int8_t> (typedef)
- ****atomic_uint8_t** (C++11)(optional)** — std::atomic<std::uint8_t> (typedef)
- ****atomic_int16_t** (C++11)(optional)** — std::atomic<std::int16_t> (typedef)
- ****atomic_uint16_t** (C++11)(optional)** — std::atomic<std::uint16_t>
  (typedef)
- ****atomic_int32_t** (C++11)(optional)** — std::atomic<std::int32_t> (typedef)
- ****atomic_uint32_t** (C++11)(optional)** — std::atomic<std::uint32_t>
  (typedef)
- ****atomic_int64_t** (C++11)(optional)** — std::atomic<std::int64_t> (typedef)
- ****atomic_uint64_t** (C++11)(optional)** — std::atomic<std::uint64_t>
  (typedef)
- ****atomic_int_least8_t** (C++11)** — std::atomic<std::int_least8_t> (typedef)
- ****atomic_uint_least8_t** (C++11)** — std::atomic<std::uint_least8_t>
  (typedef)
- ****atomic_int_least16_t** (C++11)** — std::atomic<std::int_least16_t>
  (typedef)
- ****atomic_uint_least16_t** (C++11)** — std::atomic<std::uint_least16_t>
  (typedef)
- ****atomic_int_least32_t** (C++11)** — std::atomic<std::int_least32_t>
  (typedef)
- ****atomic_uint_least32_t** (C++11)** — std::atomic<std::uint_least32_t>
  (typedef)
- ****atomic_int_least64_t** (C++11)** — std::atomic<std::int_least64_t>
  (typedef)
- ****atomic_uint_least64_t** (C++11)** — std::atomic<std::uint_least64_t>
  (typedef)
- ****atomic_int_fast8_t** (C++11)** — std::atomic<std::int_fast8_t> (typedef)
- ****atomic_uint_fast8_t** (C++11)** — std::atomic<std::uint_fast8_t> (typedef)
- ****atomic_int_fast16_t** (C++11)** — std::atomic<std::int_fast16_t> (typedef)
- ****atomic_uint_fast16_t** (C++11)** — std::atomic<std::uint_fast16_t>
  (typedef)
- ****atomic_int_fast32_t** (C++11)** — std::atomic<std::int_fast32_t> (typedef)
- ****atomic_uint_fast32_t** (C++11)** — std::atomic<std::uint_fast32_t>
  (typedef)
- ****atomic_int_fast64_t** (C++11)** — std::atomic<std::int_fast64_t> (typedef)
- ****atomic_uint_fast64_t** (C++11)** — std::atomic<std::uint_fast64_t>
  (typedef)
- ****atomic_intptr_t** (C++11)(optional)** — std::atomic<std::intptr_t>
  (typedef)
- ****atomic_uintptr_t** (C++11)(optional)** — std::atomic<std::uintptr_t>
  (typedef)
- ****atomic_size_t** (C++11)** — std::atomic<std::size_t> (typedef)
- ****atomic_ptrdiff_t** (C++11)** — std::atomic<std::ptrdiff_t> (typedef)
- ****atomic_intmax_t** (C++11)** — std::atomic<std::intmax_t> (typedef)
- ****atomic_uintmax_t** (C++11)** — std::atomic<std::uintmax_t> (typedef)

**Aliases for special-purpose types**

- ****atomic_signed_lock_free** (C++20)** — a signed integral atomic type that
  is lock-free and for which waiting/notifying is most efficient (typedef)
- ****atomic_unsigned_lock_free** (C++20)** — an unsigned integral atomic type
  that is lock-free and for which waiting/notifying is most efficient (typedef)

Note: `std::atomic_intN_t`, `std::atomic_uintN_t`, `std::atomic_intptr_t`, and
`std::atomic_uintptr_t` are defined if and only if `std::intN_t`,
`std::uintN_t`, `std::intptr_t`, and `std::uintptr_t` are defined, respectively.

`std::atomic_signed_lock_free` and `std::atomic_unsigned_lock_free` are optional
in freestanding implementations.
*(since C++20)*

### Member types

- **`value_type`** — `T` (regardless of whether specialized or not)
- **`difference_type`** — `value_type` (only for `atomic<Integral>` and
  `atomic<Floating>`(since C++20) specializations) `std::ptrdiff_t` (only for
  `std::atomic<U*>` specializations)

`difference_type` is not defined in the primary `std::atomic` template or in the
partial specializations for `std::shared_ptr` and `std::weak_ptr`.

### Member functions

- **(constructor)** — constructs an atomic object (public member function)
- **operator=** — stores a value into an atomic object (public member function)
- **is_lock_free** — checks if the atomic object is lock-free (public member
  function)
- **store** — atomically replaces the value of the atomic object with a
  non-atomic argument (public member function)
- **load** — atomically obtains the value of the atomic object (public member
  function)
- **operator T** — loads a value from an atomic object (public member function)
- **exchange** — atomically replaces the value of the atomic object and obtains
  the value held previously (public member function)
- **compare_exchange_weakcompare_exchange_strong** — atomically compares the
  value of the atomic object with non-atomic argument and performs atomic
  exchange if equal or atomic load if not (public member function)
- **wait (C++20)** — blocks the thread until notified and the atomic value
  changes (public member function)
- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function)

**Constants**

- **is_always_lock_free [static] (C++17)** — indicates that the type is always
  lock-free (public static member constant)

### Specialized member functions

- **fetch_add** — atomically adds the argument to the value stored in the atomic
  object and obtains the value held previously (public member function)
- **fetch_sub** — atomically subtracts the argument from the value stored in the
  atomic object and obtains the value held previously (public member function)
- **fetch_and** — atomically performs bitwise AND between the argument and the
  value of the atomic object and obtains the value held previously (public
  member function)
- **fetch_or** — atomically performs bitwise OR between the argument and the
  value of the atomic object and obtains the value held previously (public
  member function)
- **fetch_xor** — atomically performs bitwise XOR between the argument and the
  value of the atomic object and obtains the value held previously (public
  member function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the atomic value by one (public member function)
- **operator+=operator-=operator&=operator|=operator^=** — adds, subtracts, or
  performs bitwise AND, OR, XOR with the atomic value (public member function)

### Notes

There are non-member function template equivalents for all member functions of
`std::atomic`. Those non-member functions may be additionally overloaded for
types that are not specializations of `std::atomic`, but are able to guarantee
atomicity. The only such type in the standard library is std::shared_ptr<U>.

`_Atomic` is a keyword and used to provide atomic types in C.

Implementations are recommended to ensure that the representation of
`_Atomic(T)` in C is same as that of `std::atomic<T>` in C++ for every possible
type `T`. The mechanisms used to ensure atomicity and memory ordering should be
compatible.

On gcc and clang, some of the functionality described here requires linking
against `-latomic`.

### Example

```cpp
#include <atomic>
#include <iostream>
#include <thread>
#include <vector>

std::atomic_int acnt;
int cnt;

void f()
{
    for (int n = 0; n < 10000; ++n)
    {
        ++acnt;
        ++cnt;
        // Note: for this example, relaxed memory order
        // is sufficient, e.g. acnt.fetch_add(1, std::memory_order_relaxed);
    }
}

int main()
{
    {
        std::vector<std::jthread> pool;
        for (int n = 0; n < 10; ++n)
            pool.emplace_back(f);
    }

    std::cout << "The atomic counter is " << acnt << '\n'
              << "The non-atomic counter is " << cnt << '\n';
}
```

Possible output:

```text
The atomic counter is 100000
The non-atomic counter is 69696
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2441 | C++11 | typedefs for atomic versions of optional fixed width
      integer types were missing | added
  LWG 3012 | C++11 | `std::atomic<T>` was permitted for any `T` that is
      trivially copyable but not copyable | such specializations are forbidden
  P0558R1 | C++11 | template argument deduction for some functions for atomic
      types might accidently fail; invalid pointer operations were provided |
      specification was substantially rewritten: member typedefs `value_type`
      and `difference_type` are added

### See also

- **atomic_flag (C++11)** — the lock-free boolean atomic type (class)
- **std::atomic<std::shared_ptr> (C++20)** — atomic shared pointer (class
  template specialization)
- **std::atomic<std::weak_ptr> (C++20)** — atomic weak pointer (class template
  specialization)

**C documentation for Atomic types**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic*
