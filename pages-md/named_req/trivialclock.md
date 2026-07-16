# C++ named requirements: TrivialClock (since C++11)

The **TrivialClock** requirements describe the requirements satisfied by several
clocks in the chrono library.

### Requirements

For a type `TC`:

- The type must meet Clock requirements.
- The types `TC::rep`, `TC::duration`, and `TC::time_point` satisfy the
  requirements of EqualityComparable, LessThanComparable, DefaultConstructible,
  CopyConstructible, CopyAssignable, Destructible, and NumericType.
- lvalues of the types `TC::rep`, `TC::duration`, and `TC::time_point` are
  Swappable.
- The function `TC::now()` does not throw exceptions.
- The type `TC::time_point::clock` meets the TrivialClock requirements,
  recursively.

### Usage

The following types in the standard library satisfy these requirements:

- `std::chrono::system_clock`
- `std::chrono::steady_clock`
- `std::chrono::high_resolution_clock`
- `std::filesystem::file_time_type::clock`
- `std::chrono::file_clock`

---
*Source: https://en.cppreference.com/w/cpp/named_req/TrivialClock*
