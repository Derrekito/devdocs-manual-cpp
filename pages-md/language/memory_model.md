# Memory model

Defines the semantics of computer memory storage for the purpose of the C++
abstract machine.

The memory available to a C++ program is one or more contiguous sequences of
*bytes*. Each byte in memory has a unique *address*.

### Byte

A *byte* is the smallest addressable unit of memory. It is defined as a
contiguous sequence of bits, large enough to hold

- the value of any `UTF-8` code unit (256 distinct values) and of

- any member of the basic execution character set.
*(until C++23)*

- the ordinary literal encoding of any element of the basic literal character
  set.
*(since C++23)*

Similar to C, C++ supports bytes of sizes 8 bits and greater.

The types `char`, `unsigned char`, and `signed char` use one byte for both
storage and value representation. The number of bits in a byte is accessible as
`CHAR_BIT` or `std::numeric_limits<unsigned char>::digits`.

### Memory location

A *memory location* is

- an object of scalar type (arithmetic type, pointer type, enumeration type, or
  `std::nullptr_t`), or
- the largest contiguous sequence of bit-fields of non-zero length.

Note: Various features of the language, such as references and virtual
functions, might involve additional memory locations that are not accessible to
programs but are managed by the implementation.

```cpp
struct S
{
    char a;     // memory location #1
    int b : 5;  // memory location #2
    int c : 11, // memory location #2 (continued)
          : 0,
        d : 8;  // memory location #3
    struct
    {
        int ee : 8; // memory location #4
    } e;
} obj; // The object 'obj' consists of 4 separate memory locations
```

### Threads and data races

A thread of execution is a flow of control within a program that begins with the
invocation of a top-level function by `std::thread::thread`, `std::async`, or
other means.

Any thread can potentially access any object in the program (objects with
automatic and thread-local storage duration may still be accessed by another
thread through a pointer or by reference).

Different threads of execution are always allowed to access (read and modify)
different *memory locations* concurrently, with no interference and no
synchronization requirements.

When an evaluation of an expression modifies a memory location and another
evaluation reads or modifies the same memory location, the expressions are said
to *conflict*. A program that has two conflicting evaluations has a *data race*
unless

- both evaluations execute on the same thread or in the same signal handler, or
- both conflicting evaluations are atomic operations (see `std::atomic`), or
- one of the conflicting evaluations *happens-before* another (see
  `std::memory_order`).

If a data race occurs, the behavior of the program is undefined.

(In particular, release of a `std::mutex` is *synchronized-with*, and therefore,
*happens-before* acquisition of the same mutex by another thread, which makes it
possible to use mutex locks to guard against data races.)

```cpp
int cnt = 0;
auto f = [&] { cnt++; };
std::thread t1{f}, t2{f}, t3{f}; // undefined behavior
```

```cpp
std::atomic<int> cnt{0};
auto f = [&] { cnt++; };
std::thread t1{f}, t2{f}, t3{f}; // OK
```

### Memory order

When a thread reads a value from a memory location, it may see the initial
value, the value written in the same thread, or the value written in another
thread. See `std::memory_order` for details on the order in which writes made
from threads become visible to other threads.

### Forward progress

#### Obstruction freedom

When only one thread that is not blocked in a standard library function executes
an atomic function that is lock-free, that execution is guaranteed to complete
(all standard library lock-free operations are obstruction-free).

#### Lock freedom

When one or more lock-free atomic functions run concurrently, at least one of
them is guaranteed to complete (all standard library lock-free operations are
lock-free — it is the job of the implementation to ensure they cannot be
live-locked indefinitely by other threads, such as by continuously stealing the
cache line).

#### Progress guarantee

In a valid C++ program, every thread eventually does one of the following:

- terminate;
- makes a call to an I/O library function;
- performs an access through a volatile glvalue;
- performs an atomic operation or a synchronization operation.

This allows the compilers to remove all loops that have no observable behavior,
without having to prove that they would eventually terminate because it can
assume that no thread of execution can execute forever without performing any of
these observable behaviors.

A thread is said to *make progress* if it performs one of the execution steps
above (I/O, volatile, atomic, or synchronization), blocks in a standard library
function, or calls an atomic lock-free function that does not complete because
of a non-blocked concurrent thread.

#### Concurrent forward progress
If a thread offers *concurrent forward progress guarantee*, it will *make
progress* (as defined above) in finite amount of time, for as long as it has not
terminated, regardless of whether other threads (if any) are making progress.
The standard encourages, but doesn't require that the main thread and the
threads started by `std::thread` offer concurrent forward progress guarantee.
#### Parallel forward progress
If a thread offers *parallel forward progress guarantee*, the implementation is
not required to ensure that the thread will eventually make progress if it has
not yet executed any execution step (I/O, volatile, atomic, or synchronization),
but once this thread has executed a step, it provides *concurrent forward
progress* guarantees (this rule describes a thread in a thread pool that
executes tasks in arbitrary order).
#### Weakly parallel forward progress
If a thread offers *weakly parallel forward progress guarantee*, it does not
guarantee to eventually make progress, regardless of whether other threads make
progress or not.
Such threads can still be guaranteed to make progress by blocking with forward
progress guarantee delegation: if a thread P blocks in this manner on the
completion of a set of threads S, then at least one thread in S will offer a
forward progress guarantee that is same or stronger than P. Once that thread
completes, another thread in S will be similarly strengthened. Once the set is
empty, P will unblock.
The parallel algorithms from the C++ standard library block with forward
progress delegation on the completion of an unspecified set of library-managed
threads.
*(since C++17)*

### See also

**C documentation for Memory model**

---
*Source: https://en.cppreference.com/w/cpp/language/memory_model*
