# C++ named requirements: RandomNumberEngineAdaptor (since C++11)

A **RandomNumberEngineAdaptor** is a RandomNumberEngine that transforms the
output of another RandomNumberEngine, the latter of which is termed a *base
engine*.

### Requirements

Given

- `B`, a RandomNumberEngine
- `b`, an object of type `B`
- `A`, a **RandomNumberEngineAdaptor**, adapting `B` as a base engine
- `a`, an object of type `A`

  function | semantics
  `A::A();` | The base engine is initialized as if by its default constructor.
  `bool operator==(const A& lhs, const A& rhs);` | Returns whether `lhs`'s base
      engine is equal to `rhs`'s base engine or not.
  `A::A(result_type s);` | The base engine is initialized with `s`.
  `template<class Sseq> A::A(Sseq& q);` | The base engine is initialized with
      `q`.
  `void seed();` | Invokes `b.seed()`.
  `void seed(result_type s);` | Invokes `b.seed(s)`.
  `template<class Sseq> void seed(Sseq& q);` | Invokes `b.seed(q)`.

Furthermore

- The complexity of each function of `A` does not exceed the complexity of the
  function of `B`
- The state of `A` includes the state of `B`
- The size of `A`'s state is no less than the size of `b`
- Copying `A`'s state includes copying `B`'s state
- The textual representation of `A` includes the textual representation of `B`

### Standard library

The following standard library facilities satisfy **RandomNumberEngineAdaptor**:

- **discard_block_engine (C++11)** — discards some output of a random number
  engine (class template)
- **independent_bits_engine (C++11)** — packs the output of a random number
  engine into blocks of a specified number of bits (class template)
- **shuffle_order_engine (C++11)** — delivers the output of a random number
  engine in a different order (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/RandomNumberEngineAdaptor*
