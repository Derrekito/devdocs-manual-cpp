# Utility library

C++ includes a variety of utility libraries that provide functionality ranging
from bit-counting to partial function application. These libraries can be
broadly divided into two groups:

- language support libraries, and
- general-purpose libraries.

## Language support

Language support libraries provide classes and functions that interact closely
with language features and support common language idioms.

### Type support

Basic types (e.g. `std::size_t`, `std::nullptr_t`), RTTI (e.g.
`std::type_info`), type traits (e.g. `std::is_integral`, `std::rank`)

### Constant evaluation context

- **is_constant_evaluated (C++20)** — detects whether the call occurs within a
  constant-evaluated context (function)
- **is_within_lifetime (C++26)** — checks whether a pointer is within the
  object's lifetime at compile time (function)

### Implementation properties

The header `<version>` supplies implementation-dependent information about the
C++ standard library (such as the version number and release date). It also
defines the library feature-test macros.
*(since C++20)*

### Program utilities

Termination (e.g. `std::abort`, `std::atexit`), environment (e.g.
`std::system`), signals (e.g. `std::raise`)

### Dynamic memory management

Smart pointers (e.g. `std::shared_ptr`), allocators (e.g. `std::allocator` or
`std::pmr::memory_resource`), C-style memory management (e.g. `std::malloc`)

### Error handling

Exceptions (e.g. `std::exception`, `std::terminate`), assertions (e.g. `assert`)

### Source code information capture

- **source_location (C++20)** — a class representing information about the
  source code, such as file names, line numbers, and function names (class)

### Debugging support

- **breakpoint (C++26)** — pauses the running program when called (function)
- **breakpoint_if_debugging (C++26)** — calls `std::breakpoint` if
  `std::is_debugger_present` returns `true` (function)
- **is_debugger_present (C++26)** — checks whether a program is running under
  the control of a debugger (function)

### Initializer lists

- **initializer_list (C++11)** — creates a temporary array in
  list-initialization and then references it (class template)

### Three-way comparison

- **three_way_comparablethree_way_comparable_with (C++20)** — specifies that
  operator `<=>` produces consistent result on given types (concept)
- **partial_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators, is not substitutable, and allows incomparable values
  (class)
- **weak_ordering (C++20)** — the result type of 3-way comparison that supports
  all 6 operators and is not substitutable (class)
- **strong_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators and is substitutable (class)
- **is_eqis_neqis_ltis_lteqis_gtis_gteq (C++20)** — named comparison functions
  (function)
- **compare_three_way (C++20)** — function object implementing `x <=> y` (class)
- **compare_three_way_result (C++20)** — obtains the result type of the
  three-way comparison operator `<=>` on given types (class template)
- **common_comparison_category (C++20)** — the strongest comparison category to
  which all of the given types can be converted (class template)
- **strong_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::strong_ordering` (customization point object)
- **weak_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::weak_ordering` (customization point object)
- **partial_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::partial_ordering` (customization point object)
- **compare_strong_order_fallback (C++20)** — performs 3-way comparison and
  produces a result of type `std::strong_ordering`, even if `operator<=>` is
  unavailable (customization point object)
- **compare_weak_order_fallback (C++20)** — performs 3-way comparison and
  produces a result of type `std::weak_ordering`, even if `operator<=>` is
  unavailable (customization point object)
- **compare_partial_order_fallback (C++20)** — performs 3-way comparison and
  produces a result of type `std::partial_ordering`, even if `operator<=>` is
  unavailable (customization point object)

### Coroutine support

Types for coroutine support, e.g. `std::coroutine_traits`,
`std::coroutine_handle`.
*(since C++20)*

### Variadic functions

Support for functions that take an arbitrary number of parameters (via e.g.
`va_start`, `va_arg`, `va_end`).

## General-purpose utilities

### Swap

- **swap** — swaps the values of two objects (function template)
- **exchange (C++14)** — replaces the argument with a new value and returns its
  previous value (function template)
- **ranges::swap (C++20)** — swaps the values of two objects (customization
  point object)

### Type operations

- **forward (C++11)** — forwards a function argument (function template)
- **forward_like (C++23)** — forwards a function argument as if casting it to
  the value category and constness of the expression of specified type template
  argument (function template)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)
- **declval (C++11)** — obtains a reference to its argument for use in
  unevaluated context (function template)
- **to_underlying (C++23)** — converts an enumeration to its underlying type
  (function template)

### Integer comparison functions

- **cmp_equalcmp_not_equalcmp_lesscmp_greatercmp_less_equalcmp_greater_equal
  (C++20)** — compares two integer values without value change caused by
  conversion (function template)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)

### Relational operators

- **operator!=operator>operator<=operator>= (deprecated in C++20)** —
  automatically generates comparison operators based on user-defined
  `operator==` and `operator<` (function template)

### Pairs and tuples

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **piecewise_constructpiecewise_construct_t (C++11)** — piecewise construction
  tag (tag)
- **integer_sequence (C++14)** — implements compile-time sequence of integers
  (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)
- **make_from_tuple (C++17)** — construct an object with a tuple of arguments
  (function template)

**Tuple protocol**

- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)

### Sum types and type erased wrappers

- **optional (C++17)** — a wrapper that may or may not hold an object (class
  template)
- **expected (C++23)** — a wrapper that contains either an expected or error
  value (class template)
- **variant (C++17)** — a type-safe discriminated union (class template)
- **any (C++17)** — objects that hold instances of any CopyConstructible type
  (class)
- **in_placein_place_typein_place_indexin_place_tin_place_type_tin_place_index_t
  (C++17)** — in-place construction tag (tag)

### Bitset

- **bitset** — implements constant length bit array (class template)

### Function objects

Partial function application (e.g. `std::bind`) and related utilities: utilities
for binding such as `std::ref` and `std::placeholders`, polymorphic function
wrappers: `std::function`, predefined functors (e.g. `std::plus`,
`std::equal_to`), pointer-to-member to function converters `std::mem_fn`.

### Hash support

- **hash (C++11)** — hash function object (class template)

### Date and time

Time tracking (e.g. `std::chrono::time_point`, `std::chrono::duration`), C-style
date and time (e.g. `std::time`, `std::clock`)

### Elementary string conversions

In addition to sophisticated locale-dependent parsers and formatters provided by
the C++ I/O library, the C I/O library, C++ string converters, and C string
converters, the header `<charconv>` provides light-weight, locale-independent,
non-allocating, non-throwing parsers and formatters for arithmetic types.

- **to_chars (C++17)** — converts an integer or floating-point value to a
  character sequence (function)
- **to_chars_result (C++17)** — the return type of `std::to_chars` (class)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **from_chars_result (C++17)** — the return type of `std::from_chars` (class)
- **chars_format (C++17)** — specifies formatting for `std::to_chars` and
  `std::from_chars` (enum)

### Formatting library

Facilities for type-safe string formatting.

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **format_to (C++20)** — writes out formatted representation of its arguments
  through an output iterator (function template)
- **format_to_n (C++20)** — writes out formatted representation of its arguments
  through an output iterator, not exceeding specified size (function template)
- **formatted_size (C++20)** — determines the number of characters necessary to
  store the formatted representation of its arguments (function template)
- **vformat (C++20)** — non-template variant of `std::format` using type-erased
  argument representation (function)
- **vformat_to (C++20)** — non-template variant of `std::format_to` using
  type-erased argument representation (function template)
- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **format_error (C++20)** — exception type thrown on formatting errors (class)

### See also

**C documentation for Utility library**

---
*Source: https://en.cppreference.com/w/cpp/utility*
