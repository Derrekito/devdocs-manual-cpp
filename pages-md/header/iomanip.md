# Standard library header <iomanip>

`<iomanip>` holds the *parameterized* stream manipulators — the ones
that take an argument, like `std::setw(n)`, as opposed to the plain
ones (`std::endl`, `std::fixed`, …) that live directly in
`<iostream>`/`<ios>`. Include it whenever you call `setw`, `setfill`,
`setprecision`, or the money/time/quoted helpers below.

- **`resetiosflags`** — clear specific `ios_base` format flags
- **`setiosflags`** — set specific `ios_base` format flags
- **`setbase`** — set the base used for integer I/O (8, 10, or 16)
- **`setfill`** — set the character used to pad fields
- **`setprecision`** — set floating-point precision, sticky until
  changed
- **`setw`** — set the width of the *next* field only, then it resets
- **`get_money`** / **`put_money`** (C++11) — parse / format a
  monetary value
- **`get_time`** / **`put_time`** (C++11) — parse / format a
  date-time value against a `strftime`-style format string
- **`quoted`** (C++14) — insert/extract a string with embedded spaces,
  wrapped in quotes with escaping

---
*Source: https://en.cppreference.com/w/cpp/header/iomanip*
