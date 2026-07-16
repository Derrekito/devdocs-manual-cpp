# std::independent_bits_engine

```cpp
template<
    class Engine,
    std::size_t W,
    class UIntType
> class independent_bits_engine;  // (since C++11)
```

`independent_bits_engine` is a random number engine adaptor that produces random
numbers with different number of bits than that of the wrapped engine.

### Template parameters

- **Engine** — the type of the wrapped engine
- **W** — the number of bits the generated numbers should have
- **UIntType** — the type of the generated random numbers. The effect is
  undefined unless the parameter is cv-unqualified and is one of `unsigned
  short`, `unsigned int`, `unsigned long`, or `unsigned long long`.

**Type requirements**

**-`Engine` must meet the requirements of RandomNumberEngine.**

**-`W` must be greater than zero, and no greater than `std::numeric_limits<UIntType>::digits`.**

### Member types

- **`result_type` (C++11)** — `UIntType`

### Member functions

- **(constructor) (C++11)** — constructs the engine adaptor (public member
  function)
- **seed (C++11)** — sets the state of the underlying engine (public member
  function)
- **base (C++11)** — returns the underlying engine (public member function)

**Generation**

- **operator() (C++11)** — advances the state of the underlying engine and
  returns the generated value (public member function)
- **discard (C++11)** — advances the adaptor's state by a specified amount
  (public member function)

**Characteristics**

- **min [static] (C++11)** — gets the smallest possible value in the output
  range (always zero). (public static member function)
- **max [static] (C++11)** — gets the largest possible value in the output range
  (always 2w - 1). (public static member function)

### Non-member functions

- **operator==operator!= (C++11)(C++11)(removed in C++20)** — compares the
  internal states of the adaptors and underlying engines (function)
- **operator<<operator>> (C++11)** — performs stream input and output on
  pseudo-random number engine adaptor (function)

### Example

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/independent_bits_engine*
