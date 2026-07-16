# std::endl

Inserts `'\n'` **and then flushes** the stream — it is `os.put('\n')`
followed by `os.flush()`, not just a newline. That extra flush is the
whole story: it forces buffered output out immediately, which is
useful but not free.

```cpp skip
out << std::endl;   // '\n', then an explicit flush
```

### What you provide

- **os** — the output stream (via `out << std::endl`); works for any
  `std::basic_ostream`.

### Guarantees and costs

- Equivalent to `os.put(os.widen('\n')); os.flush();`.
- The flush is a real cost against file streams and anything without
  automatic flushing — it forces a write syscall instead of batching.
- In a loop, prefer plain `'\n'` and flush once when the loop
  finishes (or not at all); using `endl` per iteration can noticeably
  degrade output performance, exactly the pattern to avoid.
- Flushing matters when output must appear right away: progress from
  a long-running process, interleaved logging from multiple threads,
  or before `std::system` if the spawned process does screen I/O.
- In ordinary interactive use with `std::cout`, `endl` is often
  redundant: reading from `std::cin`, writing to `std::cerr`, or
  program termination already forces a `std::cout.flush()`.
- On many implementations standard output is line-buffered and a
  bare `'\n'` flushes anyway, unless
  `std::ios::sync_with_stdio(false)` was called — in that case an
  unnecessary `endl` mainly costs file output, not console output.

### Gotchas

- Reflexively using `endl` instead of `'\n'` everywhere is a common
  and measurable performance mistake, especially writing to files.
- `endl` only flushes `os`; it does not flush other streams.
- Need to flush without adding a newline? Use `std::flush` instead.

### Example

```cpp
#include <iostream>

int main()
{
    // Loop: '\n' only, flush once when done.
    for (int i = 1; i <= 3; ++i)
        std::cout << "line " << i << '\n';
    std::cout << std::flush;

    // One-off: endl is fine here.
    std::cout << "done" << std::endl;
}
```

```text
line 1
line 2
line 3
done
```

### Reference

```cpp skip
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    endl( std::basic_ostream<CharT, Traits>& os );
```

Returns `os`.

### See also

- **flush** — flushes the stream without inserting a newline
- **unitbuf**, **nounitbuf** — flush after every output operation
- **setprecision** — sticky floating-point precision
- **fixed** — the floatfield formatting group

---
*Source: https://en.cppreference.com/w/cpp/io/manip/endl*
