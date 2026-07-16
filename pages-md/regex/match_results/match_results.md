# std::match_results<BidirIt,Alloc>::match_results

```cpp
match_results() : match_results(Allocator()) {}  // (1) (since C++11)
explicit match_results( const Allocator& a );  // (2) (since C++11)
match_results( const match_results& rhs );  // (3) (since C++11)
match_results( const match_results& rhs, const Allocator& a );  // (4) (since C++11)
match_results( match_results&& rhs ) noexcept;  // (5) (since C++11)
match_results( match_results&& rhs, const Allocator& a );  // (6) (since C++11)
```

1) Default constructor. Constructs a match result with no established result
   state (`ready() != true`).

2) Constructs a match result with no established result state (`ready() !=
   true`) using a copy of `a` as the allocator.

3) Copy constructor. Constructs a match result with a copy of `rhs`.

4) Constructs a match result with a copy of `rhs`, and uses a copy of `a` as the
   allocator.

5) Move constructor. Constructs a match result with the contents of `rhs` using
   move semantics. `rhs` is in valid, but unspecified state after the call.

6) Constructs a match result with the contents of `rhs` using move semantics,
   and uses a copy of `a` as the allocator. `rhs` is in valid, but unspecified
   state after the call.

### Parameters

- **a** — allocator to use for all memory allocations of this container
- **rhs** — another `match_results` to use as source to initialize the
  `match_results` with

### Exceptions

1-4) May throw implementation-defined exceptions.

6) Throws nothing if `a == rhs.get_allocator()` is `true`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2195 | C++11 | the constructors required by AllocatorAwareContainer were
      missing | added
  P0935R0 | C++11 | default constructor was explicit | made implicit

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/match_results*
