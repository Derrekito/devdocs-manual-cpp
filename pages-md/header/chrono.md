# Standard library header <chrono> (C++11)

`<chrono>` is the date and time library, and it works in three layers.
The C++11 core — `duration`, `time_point`, and the clocks
(`system_clock`, `steady_clock`, ...) — measures elapsed time and marks
instants; reach for it whenever you're timing something or storing a
timestamp. C++20 added a calendar layer (`year`, `month`, `day`,
`year_month_day`, ...) for representing Gregorian dates with correct
arithmetic, and a time zone layer (`time_zone`, `zoned_time`, the
`tzdb` IANA database access) for converting between civil (local) time
and absolute time.

A `duration` is a `Rep` and a `Period`, and knows nothing about
calendars; a `time_point` anchors a `duration` to a specific `Clock`.
Neither has a time zone or civil-calendar meaning until you pair it
with the C++20 calendar and time-zone facilities below.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support

### Duration

- **duration** (C++11) — a time interval, as a count (`Rep`) of a tick
  period (`Period`)
- **treat_as_floating_point** (C++11) — is a duration's rep convertible
  across tick periods without a cast?
- **duration_values** (C++11) — the zero, min, and max values of a
  duration's rep type
- **nanoseconds** / **microseconds** / **milliseconds** / **seconds** /
  **minutes** / **hours** (C++11) — the small-scale duration typedefs
- **days** / **weeks** / **months** / **years** (C++20) — the
  calendar-scale duration typedefs

### Duration functions

- **operator+** / **operator-** / **operator\*** / **operator/** /
  **operator%** (C++11) — arithmetic on durations
- **operator==** and the relational operators (C++11) — compares two
  durations (`operator!=` was removed in C++20, synthesized from `==`;
  `operator<=>` added then)
- **duration_cast** (C++11) — converts a duration to another tick
  period
- **floor** / **ceil** / **round** (C++17) — converts a duration to
  another, rounding down, up, or to nearest (ties to even)
- **abs** (C++17) — the absolute value of a duration

### Time point

- **time_point** (C++11) — a point in time on a given `Clock`
- **clock_time_conversion** (C++20) — traits defining how to convert a
  time point from one clock to another

### Time point functions

- **operator+** / **operator-** (C++11) — adds or subtracts a duration
  and a time point
- **operator==** and the relational operators (C++11) — compares two
  time points (`operator!=` was removed in C++20, synthesized from
  `==`; `operator<=>` added then)
- **time_point_cast** (C++11) — converts a time point to another
  duration on the same clock
- **floor** / **ceil** / **round** (C++17) — converts a time point to
  another, rounding down, up, or to nearest (ties to even)
- **clock_cast** (C++20) — converts a time point from one clock to
  another

### Clocks

- **is_clock** / **is_clock_v** (C++20) — does a type meet the `Clock`
  requirements?
- **system_clock** (C++11) — wall-clock time from the system-wide
  realtime clock
- **steady_clock** (C++11) — a monotonic clock that is never adjusted
- **high_resolution_clock** (C++11) — the clock with the shortest tick
  period available
- **utc_clock** (C++20) — Coordinated Universal Time
- **tai_clock** (C++20) — International Atomic Time
- **gps_clock** (C++20) — GPS time
- **file_clock** (C++20) — the clock used for filesystem file times
- **local_t** (C++20) — pseudo-clock representing local (civil) time

### Calendar

- **day** (C++20) — a day of the month
- **month** (C++20) — a month of the year
- **year** (C++20) — a year in the Gregorian calendar
- **weekday** (C++20) — a day of the week
- **weekday_indexed** (C++20) — the nth `weekday` of a month
- **weekday_last** (C++20) — the last `weekday` of a month
- **last_spec** (C++20) — tag class marking "the last day/weekday" in a
  month
- **month_day** (C++20) — a specific `day` of a specific `month`
- **month_day_last** (C++20) — the last day of a specific `month`
- **month_weekday** (C++20) — the nth `weekday` of a specific `month`
- **month_weekday_last** (C++20) — the last `weekday` of a specific
  `month`
- **year_month** (C++20) — a specific `month` of a specific `year`
- **year_month_day** (C++20) — a specific `year`, `month`, and `day`
- **year_month_day_last** (C++20) — the last day of a specific `year`
  and `month`
- **year_month_weekday** (C++20) — the nth `weekday` of a specific
  `year` and `month`
- **year_month_weekday_last** (C++20) — the last `weekday` of a
  specific `year` and `month`

### Calendar comparisons and arithmetic (C++20)

Every type above supports `operator==`. The weekday-based types
(`weekday`, `weekday_indexed`, `weekday_last`, `month_weekday`,
`month_weekday_last`, `year_month_weekday`, `year_month_weekday_last`)
support only `operator==`, since a weekday has no natural strict
ordering; the rest also support `operator<=>`.

`day`, `month`, `year`, and `weekday` support `operator+`/`operator-`
against a plain count (`day` and `weekday` also support subtracting two
values of the same type). `year_month`, `year_month_day`,
`year_month_day_last`, `year_month_weekday`, and
`year_month_weekday_last` support adding or subtracting a count of
years or months. `operator/` provides the conventional
`2015y / March / 4` syntax for assembling a `year_month_day`.

### Time of day

- **hh_mm_ss** (C++20) — a time of day split into hours, minutes,
  seconds, and subseconds
- **is_am** / **is_pm** / **make12** / **make24** (C++20) — converts
  between 12-hour and 24-hour time-of-day representations

### Time zone

- **tzdb** (C++20) — a copy of the IANA time zone database
- **tzdb_list** (C++20) — a linked list of `tzdb` snapshots
- **time_zone** (C++20) — a time zone
- **time_zone_link** (C++20) — an alternative name for a time zone
- **sys_info** (C++20) — information about a time zone at a particular
  time point
- **local_info** (C++20) — information for resolving a local time to
  an absolute time
- **choose** (C++20) — how to resolve an ambiguous local time
- **zoned_traits** (C++20) — traits for the pointer type `zoned_time`
  uses to refer to a time zone
- **zoned_time** (C++20) — a time zone paired with a time point
- **nonexistent_local_time** (C++20) — thrown when a local time falls
  in a DST gap and doesn't exist
- **ambiguous_local_time** (C++20) — thrown when a local time falls in
  a DST fold and is ambiguous

### Time zone functions

- **get_tzdb** / **get_tzdb_list** / **reload_tzdb** /
  **remote_version** (C++20) — accesses and reloads the global time
  zone database
- **locate_zone** (C++20) — looks up a `time_zone` by name
- **operator==** and **operator<=>** (C++20) — compares two
  `time_zone`, `zoned_time`, or `time_zone_link` values

### Leap second

- **leap_second** (C++20) — one leap-second insertion event
- **leap_second_info** (C++20) — leap-second information for a queried
  time point
- **get_leap_second_info** (C++20) — leap-second info for a given
  `utc_time`
- **operator==** and the relational operators (C++20) — compares two
  `leap_second` values, or a `leap_second` against a `sys_time`

### Formatting, hashing, and I/O

- `std::formatter` is specialized (C++20) for every duration/time_point
  flavor, every calendar type above, `hh_mm_ss`, `sys_info`,
  `local_info`, and `zoned_time`, so they all work with `std::format`
- `std::hash` is specialized (C++26) for `duration`, `time_point`, the
  calendar types, `zoned_time`, and `leap_second`
- `std::common_type` is specialized (C++11) for `duration` and
  `time_point`
- **operator<<** is overloaded for every type above that has a
  `formatter`
- **from_stream** (C++20) — parses a duration/time_point or calendar
  value from a stream, given a format string
- **parse** (C++20) — parses a `chrono` object from a stream, for use
  with `std::format`

### Literals

- **operator""h** / **operator""min** / **operator""s** /
  **operator""ms** / **operator""us** / **operator""ns** (C++14) —
  duration literals, hours through nanoseconds
- **operator""d** (C++20) — a `std::chrono::day` literal
- **operator""y** (C++20) — a `std::chrono::year` literal

---
*Source: https://en.cppreference.com/w/cpp/header/chrono*
