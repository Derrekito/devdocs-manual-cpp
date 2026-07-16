# std::mersenne_twister_engine<UIntType,w,n,m,r,a,u,d,s,b,t,c,l,f>::mersenne_twister_engine

```cpp
mersenne_twister_engine() : mersenne_twister_engine(default_seed) {}  // (1) (since C++11)
explicit mersenne_twister_engine( result_type value );  // (2) (since C++11)
template< class Sseq >
explicit mersenne_twister_engine( Sseq& s );  // (3) (since C++11)
mersenne_twister_engine( const mersenne_twister_engine& );  // (4) (since C++11) (implicitly declared)
```

Constructs the pseudo-random number engine.

1) Default constructor. Seeds the engine with `default_seed`.

2) Constructs the engine and initializes the state (`n` values Xi of type
   `result_type`) as follows: value mod 2w is used to initialize X0 and the rest
   are initialized iteratively, for i=1-n,...,-1, each Xi is initialized to
   [f·(Xi-1xor(Xi-1rshift(w-2)))+i mod n] mod 2w

3) Constructs the engine and initializes the state by calling `s.generate(a,
   a+n*k)` where a is an array of length n*k and k is ceil(w/32) and then,
   iteratively for i=-n,...,-1, setting each element of the engine state Xi to
   (Σk-1j=0ak(i+n)+j}·232j) mod 2w, and finally if the most significant w-r bits
   of X0 are zero, and if all other Xi are zero, replacing X0 with 2w-1.

Note: initialization requirements are based on the 2002 version of Mersenne
Twister, mt19937ar.c

The overload (3) only participates in overload resolution if `Sseq` qualifies as
a SeedSequence. In particular, it is excluded from the set of candidate
functions if `Sseq` is convertible to `result_type`.

### Parameters

- **value** — seed value to use in the initialization of the internal state
- **s** — seed sequence to use in the initialization of the internal state

### Complexity

Linear in `n` (the state size).

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0935R0 | C++11 | default constructor was explicit | made implicit

### See also

- **seed (C++11)** — sets the current state of the engine (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/mersenne_twister_engine/mersenne_twister_engine*
