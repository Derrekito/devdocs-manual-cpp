# C++ named requirements: SeedSequence (since C++11)

A **SeedSequence** is an object that produces unsigned integer values i in the
range 0 ≤ i < 232 based on a consumed range of integer data.

### Requirements

- `S` is a SeedSequence type.
- `q` is an object of `S` and `r` is a potentially constant object of `S`.
- `T` is the `result_type`.
- `ib`, `ie` are LegacyInputIterators with a `value_type` of unsigned integer
  values of at least 32 bits.
- `il` is a `std::initializer_list<T>`.
- `rb`, `re` are mutable LegacyRandomAccessIterators with a `value_type` of
  unsigned integer values of at least 32 bits.
- `ob` is a LegacyOutputIterator.

  Expression | Type | Notes | Complexity
  `S::result_type` | `T` | Unsigned integer of at least 32 bits. | Compile-time
  `S()` | Creates a seed sequence with the same initial state as other
      default-constructed seed sequences of type `S`. | Constant
  `S(ib, ie)` | Creates a seed sequence with internal state depending on some or
      all supplied input bits by `[``ib``,``ie``)`. | O(ie - ib)
  `S(il)` | The same as `S(il.begin(), il.end())`.
  `q.generate(rb, re)` | `void` | Fills `[``rb``,``re``)` with 32-bit quantities
      depending on the initial supplied values and potential previous calls to
      `generate`. If `rb == re`, it does nothing. | O(re - rb)
  `r.size()` | `size_t` | The amount of 32-bit integers copied by `param`. |
      Constant
  `r.param(ob)` | `void` | Copies 32-bit values to `ob` that would reproduce the
      current state of the object if passed to a constructor of `S`. |
      O(r.size())

### See also

- `std::seed_seq`

---
*Source: https://en.cppreference.com/w/cpp/named_req/SeedSequence*
