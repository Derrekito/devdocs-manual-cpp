# Standard library header <stdatomic.h>

This header was originally in the C standard library.

This header is part of the concurrency support library.

It is unspecified whether `<stdatomic.h>` provides any declarations in namespace
`std`.

**Macros**

- **_Atomic (C++23)** — compatibility macro such that `_Atomic(T)` is identical
  to `std::atomic<T>` (function macro)
- **ATOMIC_FLAG_INIT (C++11)** — initializes an `std::atomic_flag` to `false`
  (macro constant)

**Types**

- **atomic_flag (C++11)** — the lock-free boolean atomic type (class)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)
- **atomic_bool (C++11)** — std::atomic<bool> (typedef)
- **atomic_char (C++11)** — std::atomic<char> (typedef)
- **atomic_schar (C++11)** — std::atomic<signed char> (typedef)
- **atomic_uchar (C++11)** — std::atomic<unsigned char> (typedef)
- **atomic_short (C++11)** — std::atomic<short> (typedef)
- **atomic_ushort (C++11)** — std::atomic<unsigned short> (typedef)
- **atomic_int (C++11)** — std::atomic<int> (typedef)
- **atomic_uint (C++11)** — std::atomic<unsigned int> (typedef)
- **atomic_long (C++11)** — std::atomic<long> (typedef)
- **atomic_ulong (C++11)** — std::atomic<unsigned long> (typedef)
- **atomic_llong (C++11)** — std::atomic<long long> (typedef)
- **atomic_ullong (C++11)** — std::atomic<unsigned long long> (typedef)
- **atomic_char8_t (C++20)** — std::atomic<char8_t> (typedef)
- **atomic_char16_t (C++11)** — std::atomic<char16_t> (typedef)
- **atomic_char32_t (C++11)** — std::atomic<char32_t> (typedef)
- **atomic_wchar_t (C++11)** — std::atomic<wchar_t> (typedef)
- **atomic_int8_t (C++11)(optional)** — std::atomic<std::int8_t> (typedef)
- **atomic_uint8_t (C++11)(optional)** — std::atomic<std::uint8_t> (typedef)
- **atomic_int16_t (C++11)(optional)** — std::atomic<std::int16_t> (typedef)
- **atomic_uint16_t (C++11)(optional)** — std::atomic<std::uint16_t> (typedef)
- **atomic_int32_t (C++11)(optional)** — std::atomic<std::int32_t> (typedef)
- **atomic_uint32_t (C++11)(optional)** — std::atomic<std::uint32_t> (typedef)
- **atomic_int64_t (C++11)(optional)** — std::atomic<std::int64_t> (typedef)
- **atomic_uint64_t (C++11)(optional)** — std::atomic<std::uint64_t> (typedef)
- **atomic_int_least8_t (C++11)** — std::atomic<std::int_least8_t> (typedef)
- **atomic_uint_least8_t (C++11)** — std::atomic<std::uint_least8_t> (typedef)
- **atomic_int_least16_t (C++11)** — std::atomic<std::int_least16_t> (typedef)
- **atomic_uint_least16_t (C++11)** — std::atomic<std::uint_least16_t> (typedef)
- **atomic_int_least32_t (C++11)** — std::atomic<std::int_least32_t> (typedef)
- **atomic_uint_least32_t (C++11)** — std::atomic<std::uint_least32_t> (typedef)
- **atomic_int_least64_t (C++11)** — std::atomic<std::int_least64_t> (typedef)
- **atomic_uint_least64_t (C++11)** — std::atomic<std::uint_least64_t> (typedef)
- **atomic_int_fast8_t (C++11)** — std::atomic<std::int_fast8_t> (typedef)
- **atomic_uint_fast8_t (C++11)** — std::atomic<std::uint_fast8_t> (typedef)
- **atomic_int_fast16_t (C++11)** — std::atomic<std::int_fast16_t> (typedef)
- **atomic_uint_fast16_t (C++11)** — std::atomic<std::uint_fast16_t> (typedef)
- **atomic_int_fast32_t (C++11)** — std::atomic<std::int_fast32_t> (typedef)
- **atomic_uint_fast32_t (C++11)** — std::atomic<std::uint_fast32_t> (typedef)
- **atomic_int_fast64_t (C++11)** — std::atomic<std::int_fast64_t> (typedef)
- **atomic_uint_fast64_t (C++11)** — std::atomic<std::uint_fast64_t> (typedef)
- **atomic_intptr_t (C++11)(optional)** — std::atomic<std::intptr_t> (typedef)
- **atomic_uintptr_t (C++11)(optional)** — std::atomic<std::uintptr_t> (typedef)
- **atomic_size_t (C++11)** — std::atomic<std::size_t> (typedef)
- **atomic_ptrdiff_t (C++11)** — std::atomic<std::ptrdiff_t> (typedef)
- **atomic_intmax_t (C++11)** — std::atomic<std::intmax_t> (typedef)
- **atomic_uintmax_t (C++11)** — std::atomic<std::uintmax_t> (typedef)

**Functions**

- **atomic_is_lock_free (C++11)** — checks if the atomic type's operations are
  lock-free (function template)
- **atomic_storeatomic_store_explicit (C++11)(C++11)** — atomically replaces the
  value of the atomic object with a non-atomic argument (function template)
- **atomic_loadatomic_load_explicit (C++11)(C++11)** — atomically obtains the
  value stored in an atomic object (function template)
- **atomic_exchangeatomic_exchange_explicit (C++11)(C++11)** — atomically
  replaces the value of the atomic object with non-atomic argument and returns
  the old value of the atomic (function template)
-
  **atomic_compare_exchange_weakatomic_compare_exchange_weak_explicitatomic_compare_exchange_strongatomic_compare_exchange_strong_explicit
  (C++11)(C++11)(C++11)(C++11)** — atomically compares the value of the atomic
  object with non-atomic argument and performs atomic exchange if equal or
  atomic load if not (function template)
- **atomic_fetch_addatomic_fetch_add_explicit (C++11)(C++11)** — adds a
  non-atomic value to an atomic object and obtains the previous value of the
  atomic (function template)
- **atomic_fetch_subatomic_fetch_sub_explicit (C++11)(C++11)** — subtracts a
  non-atomic value from an atomic object and obtains the previous value of the
  atomic (function template)
- **atomic_fetch_andatomic_fetch_and_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise AND with a non-atomic argument and
  obtains the previous value of the atomic (function template)
- **atomic_fetch_oratomic_fetch_or_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise OR with a non-atomic argument and
  obtains the previous value of the atomic (function template)
- **atomic_fetch_xoratomic_fetch_xor_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise XOR with a non-atomic argument and
  obtains the previous value of the atomic (function template)
- **atomic_flag_test_and_setatomic_flag_test_and_set_explicit (C++11)(C++11)** —
  atomically sets the flag to `true` and returns its previous value (function)
- **atomic_flag_clearatomic_flag_clear_explicit (C++11)(C++11)** — atomically
  sets the value of the flag to `false` (function)
- **atomic_thread_fence (C++11)** — generic memory order-dependent fence
  synchronization primitive (function)
- **atomic_signal_fence (C++11)** — fence between a thread and a signal handler
  executed in the same thread (function)

### Synopsis

```cpp
template<class T>
  using __std_atomic = std::atomic<T>;        // exposition only

#define _Atomic(T) __std_atomic<T>

#define ATOMIC_BOOL_LOCK_FREE /* see description */
#define ATOMIC_CHAR_LOCK_FREE /* see description */
#define ATOMIC_CHAR16_T_LOCK_FREE /* see description */
#define ATOMIC_CHAR32_T_LOCK_FREE /* see description */
#define ATOMIC_WCHAR_T_LOCK_FREE /* see description */
#define ATOMIC_SHORT_LOCK_FREE /* see description */
#define ATOMIC_INT_LOCK_FREE /* see description */
#define ATOMIC_LONG_LOCK_FREE /* see description */
#define ATOMIC_LLONG_LOCK_FREE /* see description */
#define ATOMIC_POINTER_LOCK_FREE /* see description */

using std::memory_order;                // see description
using std::memory_order_relaxed;        // see description
using std::memory_order_consume;        // see description
using std::memory_order_acquire;        // see description
using std::memory_order_release;        // see description
using std::memory_order_acq_rel;        // see description
using std::memory_order_seq_cst;        // see description

using std::atomic_flag;                 // see description

using std::atomic_bool;                 // see description
using std::atomic_char;                 // see description
using std::atomic_schar;                // see description
using std::atomic_uchar;                // see description
using std::atomic_short;                // see description
using std::atomic_ushort;               // see description
using std::atomic_int;                  // see description
using std::atomic_uint;                 // see description
using std::atomic_long;                 // see description
using std::atomic_ulong;                // see description
using std::atomic_llong;                // see description
using std::atomic_ullong;               // see description
using std::atomic_char8_t;              // see description
using std::atomic_char16_t;             // see description
using std::atomic_char32_t;             // see description
using std::atomic_wchar_t;              // see description
using std::atomic_int8_t;               // see description
using std::atomic_uint8_t;              // see description
using std::atomic_int16_t;              // see description
using std::atomic_uint16_t;             // see description
using std::atomic_int32_t;              // see description
using std::atomic_uint32_t;             // see description
using std::atomic_int64_t;              // see description
using std::atomic_uint64_t;             // see description
using std::atomic_int_least8_t;         // see description
using std::atomic_uint_least8_t;        // see description
using std::atomic_int_least16_t;        // see description
using std::atomic_uint_least16_t;       // see description
using std::atomic_int_least32_t;        // see description
using std::atomic_uint_least32_t;       // see description
using std::atomic_int_least64_t;        // see description
using std::atomic_uint_least64_t;       // see description
using std::atomic_int_fast8_t;          // see description
using std::atomic_uint_fast8_t;         // see description
using std::atomic_int_fast16_t;         // see description
using std::atomic_uint_fast16_t;        // see description
using std::atomic_int_fast32_t;         // see description
using std::atomic_uint_fast32_t;        // see description
using std::atomic_int_fast64_t;         // see description
using std::atomic_uint_fast64_t;        // see description
using std::atomic_intptr_t;             // see description
using std::atomic_uintptr_t;            // see description
using std::atomic_size_t;               // see description
using std::atomic_ptrdiff_t;            // see description
using std::atomic_intmax_t;             // see description
using std::atomic_uintmax_t;            // see description

using std::atomic_is_lock_free;                         // see description
using std::atomic_load;                                 // see description
using std::atomic_load_explicit;                        // see description
using std::atomic_store;                                // see description
using std::atomic_store_explicit;                       // see description
using std::atomic_exchange;                             // see description
using std::atomic_exchange_explicit;                    // see description
using std::atomic_compare_exchange_strong;              // see description
using std::atomic_compare_exchange_strong_explicit;     // see description
using std::atomic_compare_exchange_weak;                // see description
using std::atomic_compare_exchange_weak_explicit;       // see description
using std::atomic_fetch_add;                            // see description
using std::atomic_fetch_add_explicit;                   // see description
using std::atomic_fetch_sub;                            // see description
using std::atomic_fetch_sub_explicit;                   // see description
using std::atomic_fetch_or;                             // see description
using std::atomic_fetch_or_explicit;                    // see description
using std::atomic_fetch_xor;                            // see description
using std::atomic_fetch_xor_explicit;                   // see description
using std::atomic_fetch_and;                            // see description
using std::atomic_fetch_and_explicit;                   // see description
using std::atomic_flag_test_and_set;                    // see description
using std::atomic_flag_test_and_set_explicit;           // see description
using std::atomic_flag_clear;                           // see description
using std::atomic_flag_clear_explicit;                  // see description

#define ATOMIC_FLAG_INIT /* see description */

using std::atomic_thread_fence;                         // see description
using std::atomic_signal_fence;                         // see description
```

---
*Source: https://en.cppreference.com/w/cpp/header/stdatomic.h*
